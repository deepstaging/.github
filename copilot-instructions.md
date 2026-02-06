# Copilot Instructions

This is a multi-repository monorepo containing **Deepstaging** — a Roslyn-based code generation ecosystem for C#/.NET.

## Build & Test

Each repository has its own solution file:

```bash
# Build/test individual repos
dotnet build deepstaging/Deepstaging.slnx
dotnet test deepstaging/Deepstaging.slnx

dotnet build roslyn/Deepstaging.Roslyn.slnx
dotnet test roslyn/Deepstaging.Roslyn.slnx

dotnet build config/Deepstaging.Config.slnx
dotnet test config/Deepstaging.Config.slnx

# Run a single test
dotnet test --filter "FullyQualifiedName~MyTestClass.MyTestMethod"

# Run tests in a specific project
dotnet test deepstaging/src/Deepstaging.Tests
```

## Architecture

### Repository Structure

| Directory | Purpose |
|-----------|---------|
| `roslyn/` | Core Roslyn toolkit — Queries, Projections, Emit builders, Testing infrastructure |
| `deepstaging/` | Main product — Effects modules, runtime generation, analyzers |
| `config/` | Example project using Deepstaging.Roslyn (GenerateWith, AutoNotify) |
| `templates/` | `dotnet new` templates for creating new Roslyn projects |

### Roslyn Toolkit (roslyn/)

Three main APIs:

1. **Queries** — Fluent builders for finding symbols:
   ```csharp
   TypeQuery.From(compilation)
       .ThatArePublic()
       .ThatAreClasses()
       .WithAttribute("MyAttribute")
       .GetAll();
   ```

2. **Projections** — Optional/validated wrappers for nullable symbols:
   ```csharp
   symbol.GetAttribute("MyAttribute")
       .NamedArg<int>("MaxRetries")
       .OrDefault(3);
   ```

3. **Emit** — Fluent builders for generating C# code:
   ```csharp
   TypeBuilder.Class("Customer")
       .InNamespace("MyApp")
       .AddProperty("Name", "string", p => p.WithAutoPropertyAccessors())
       .Emit();
   ```

### The Projection Pattern

Roslyn projects follow this layered architecture:

```
Attributes (*.RoslynKit)
    ↓
Projection (*.Projection) ← Single source of truth for attribute interpretation
    ↓
Generators, Analyzers, CodeFixes
```

The Projection layer provides:
- **AttributeQuery** types that wrap attribute access
- **Model** types for strongly-typed attribute data
- Extension methods on `ValidSymbol<T>` for querying attributes

This enables consistent validation across generators and analyzers.

### Generator Pattern

Generators use the Emit API with Writer classes:

```
*.Generators/
├── MyGenerator.cs        # IIncrementalGenerator entry point
└── Writers/              # Emit logic using TypeBuilder, MethodBuilder, etc.
```

Writers transform projection models into generated code:
```csharp
module
    .WriteEffectsModule()
    .RegisterSourceWith(ctx, hint.Filename("Effects", name));
```

## Testing

### Test Framework

Uses **TUnit** (not xUnit/NUnit) with async assertions:
```csharp
await Assert.That(result).HasCount(2);
await Assert.That(name).IsEqualTo("Expected");
```

### Roslyn Testing

All Roslyn tests inherit from `RoslynTestBase`:

```csharp
public class MyTests : RoslynTestBase
{
    [Test]
    public async Task TestAnalyzer()
    {
        await AnalyzeWith<MyAnalyzer>(source)
            .ShouldReportDiagnostic("MY001")
            .WithSeverity(DiagnosticSeverity.Error)
            .WithMessage("*must be partial*");
    }

    [Test]
    public async Task TestGenerator()
    {
        await GenerateWith<MyGenerator>(source)
            .ShouldGenerate()
            .WithFileNamed("Customer.g.cs")
            .VerifySnapshot();
    }

    [Test]
    public async Task TestCodeFix()
    {
        await AnalyzeAndFixWith<MyAnalyzer, MyCodeFix>(source)
            .ForDiagnostic("MY001")
            .ShouldProduce(expectedSource);
    }
}
```

### Reference Configuration

Tests needing custom assembly references configure them via `ModuleInitializer`:
```csharp
[ModuleInitializer]
public static void Init() =>
    ReferenceConfiguration.AddReferencesFromTypes(typeof(MyAttribute));
```

### Snapshot Testing

Uses **Verify** for generator output. Snapshots are stored alongside tests with `.verified.txt` extension.

## Conventions

### Build Configuration

- **TreatWarningsAsErrors** is enabled globally
- **Nullable reference types** enabled
- **Central package management** via `Directory.Packages.props`
- Build outputs go to `artifacts/` directory
- Build config is modular: `build/Build.*.props` files

### Query/Emit Symmetry

The library maintains symmetry between reading and writing:
- `TypeQuery` finds types → `TypeBuilder` creates types
- `MethodQuery` finds methods → `MethodBuilder` creates methods
- `ValidSymbol<T>` wraps symbols → `ValidEmit` wraps generated code

### Projection Pattern

Use `Optional*` and `Valid*` wrappers instead of null checks:
```csharp
// Early exit pattern
if (optional.IsNotValid(out var valid))
    return;
// valid is now guaranteed non-null
```

### C# Extensions

This codebase uses C# 14 extension members (not extension methods):
```csharp
extension(ValidSymbol<INamedTypeSymbol> symbol)
{
    public ImmutableArray<MyAttributeQuery> MyAttributes() =>
        [..symbol.GetAttributes<MyAttribute>().Select(...)];
}
```

### SPDX Headers

All source files include SPDX license headers:
```csharp
// SPDX-FileCopyrightText: 2024-present Deepstaging
// SPDX-License-Identifier: RPL-1.5
```

## License

**RPL-1.5** (Reciprocal Public License) — Improvements must be shared back under the same license.
