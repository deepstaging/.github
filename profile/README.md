# Deepstaging

**Roslyn-based code generation ecosystem for C#/.NET.**

Build source generators, analyzers, and code fixes with fluent APIs and strong tooling.

## Repositories

### [deepstaging/roslyn](https://github.com/deepstaging/roslyn)

Fluent toolkit for building Roslyn source generators, analyzers, and code fixes.

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

**[Documentation](https://deepstaging.github.io/roslyn)**

## Tech Stack

- **C# / .NET 10** — Modern .NET with netstandard2.0 for analyzers
- **Roslyn** — Microsoft.CodeAnalysis for source generation

## License

**RPL-1.5** (Reciprocal Public License) — Share improvements back.

Use it, modify it, deploy it. When you deploy, share your changes under the same license.

## Links

- [Roslyn Toolkit Docs](https://deepstaging.github.io/roslyn)
- [Discussions](https://github.com/orgs/deepstaging/discussions)
