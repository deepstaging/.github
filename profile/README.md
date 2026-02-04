# Deepstaging

**Turn service interfaces into composable, instrumented effects — zero boilerplate.**

Deepstaging generates effect wrappers for your services at compile time, giving you functional composition with LINQ syntax, built-in OpenTelemetry tracing, and compile-time validation.

## Repositories

### [deepstaging/deepstaging](https://github.com/deepstaging/deepstaging)

The main library. Define your services, mark them with attributes, and get composable effects with tracing.

```bash
dotnet add package Deepstaging
```

```csharp
// Define services
public interface IEmailService
{
    Task SendAsync(string to, string subject, string body);
}

public interface ISlackService
{
    Task PostMessageAsync(string channel, string message);
}

// Create an effects module
[EffectsModule(typeof(IEmailService), Name = "Email")]
[EffectsModule(typeof(ISlackService), Name = "Slack")]
public partial class AppEffects;

// Define a runtime
[Runtime]
[Uses(typeof(AppEffects))]
public partial class AppRuntime;

// Compose with LINQ
using static AppEffects;

var workflow = 
    from _ in Email.SendAsync("user@example.com", "Hello", "Welcome!")
    from _ in Slack.PostMessageAsync("#general", "New user signed up!")
    select unit;

await workflow.RunAsync(runtime);
```

**Features:** Zero reflection • OpenTelemetry tracing • LanguageExt integration • Compile-time analyzers

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

## Tech Stack

- **C# / .NET 9** — Modern .NET with netstandard2.0 for analyzers
- **Roslyn** — Microsoft.CodeAnalysis for source generation
- **LanguageExt** — Functional programming primitives
- **OpenTelemetry** — Distributed tracing

## License

**RPL-1.5** (Reciprocal Public License) — Share improvements back.

Use it, modify it, deploy it. When you deploy, share your changes under the same license.

## Links

- [Deepstaging Docs](https://github.com/deepstaging/deepstaging#readme)
- [Roslyn Toolkit Docs](https://github.com/deepstaging/roslyn#readme)
- [Discussions](https://github.com/orgs/deepstaging/discussions)
