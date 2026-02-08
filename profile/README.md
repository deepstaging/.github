# Deepstaging

**Roslyn-based code generation ecosystem for C#/.NET.**

Build source generators, analyzers, and code fixes with fluent APIs and strong tooling.

## Repositories

### [deepstaging/roslyn](https://github.com/deepstaging/roslyn)

Fluent toolkit for building Roslyn source generators, analyzers, and code fixes. This powers Deepstaging's generators.

```bash
dotnet add package Deepstaging.Roslyn
```

```csharp
// Find symbols with fluent queries
var types = TypeQuery.From(compilation)
    .ThatArePublic()
    .ThatAreClasses()
    .WithAttribute("GenerateAttribute")
    .GetAll();

// Generate code with builders
var code = TypeBuilder.Class("CustomerDto")
    .InNamespace("MyApp.Models")
    .AddProperty("Id", "int", p => p.WithGetter().WithSetter())
    .Emit();
```

**Packages:** `Deepstaging.Roslyn` • `Deepstaging.Roslyn.Scriban` • `Deepstaging.Roslyn.Testing`

### [deepstaging/ids](https://github.com/deepstaging/ids)

Strongly-typed ID source generator. Generate type-safe wrappers around primitives with equality, comparison, and serialization.

```bash
dotnet add package Deepstaging.Ids
```

```csharp
[StrongId]
public readonly partial struct UserId;

[StrongId(BackingType = BackingType.Int)]
public readonly partial struct OrderId;

// Type-safe IDs with full equality, comparison, and parsing
var userId = new UserId(Guid.NewGuid());
var orderId = new OrderId(42);
```

**Features:** Guid/Int/Long/String backing types • JSON/EF Core/Dapper converters • Compile-time analyzers

### [deepstaging/templates](https://github.com/deepstaging/templates)

`dotnet new` templates for creating Roslyn projects with Deepstaging.Roslyn.

```bash
dotnet new install Deepstaging.Templates
dotnet new roslyn-generator -n MyGenerator
```

## Tech Stack

- **C# / .NET 9** — Modern .NET with netstandard2.0 for analyzers
- **Roslyn** — Microsoft.CodeAnalysis for source generation
- **LanguageExt** — Functional programming primitives
- **OpenTelemetry** — Distributed tracing

## License

**RPL-1.5** (Reciprocal Public License) — Share improvements back.

Use it, modify it, deploy it. When you deploy, share your changes under the same license.

## Links

- [Roslyn Toolkit Docs](https://deepstaging.github.io/roslyn)
- [Discussions](https://github.com/orgs/deepstaging/discussions)
