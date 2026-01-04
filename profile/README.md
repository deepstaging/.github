# Deepstaging

**Query-first infrastructure for building Roslyn source generators, analyzers, and code transformations.**

Stop fighting with Roslyn's symbol APIs. Start building reliable tooling with confidence.

---

## ✨ What Makes Deepstaging Different

Most Roslyn tutorials dump you straight into `IIncrementalGenerator` and wish you luck. We take a different approach:

**Build your queries first. Test them fast. Compose them fearlessly.**

```csharp
// Find all methods that return Task and have specific attributes
var effectMethods = compilation
    .QueryMethods()
    .Where(m => m.ReturnsTask())
    .WithAttribute("EffectAttribute")
    .Project(m => new EffectInfo(
        m.Name,
        m.Parameters.Select(p => p.Type),
        m.ReturnType
    ));

// Test in milliseconds, not seconds
Assert.That(effectMethods.Count(), Is.EqualTo(5));
```

Once your queries work, building generators is straightforward. No more debugging symbol APIs in slow integration tests.

---

## 🚀 Quick Example: Build a Generator in 5 Minutes

**1. Query for what you need:**
```csharp
var controllers = compilation
    .QueryTypes()
    .WithAttribute("ApiControllerAttribute")
    .Where(t => t.InheritsFrom("ControllerBase"))
    .Project(t => new {
        Name = t.Name,
        Actions = t.Methods()
                    .WithAttribute("HttpGetAttribute", "HttpPostAttribute")
                    .Select(m => m.Name)
    });
```

**2. Render with templates:**
```csharp
context.AddTemplate("ApiClient", new { Controllers = controllers });
```

**3. Ship it:**
```csharp
// Generated: MyApiClient.g.cs
public class MyApiClient {
    public Task<Response> GetUsers() => ...
    public Task<Response> CreateUser(User user) => ...
}
```

**That's it.** No fighting with `ISymbol` hierarchies. No mysterious null references. Just queries that work.

---

## 💎 Real-World Example: Effects System

Our [effects repository](https://github.com/deepstaging/effects) shows a complete production-ready system built on Deepstaging:

```csharp
[Effects]
public class UserService {
    [Effect("database", "write")]
    public async Task CreateUser(User user) { ... }
    
    [Effect("cache", "invalidate")]
    public async Task InvalidateUserCache(string userId) { ... }
}
```

**The generator automatically produces:**
- Effect group configurations for dependency injection
- OpenTelemetry instrumentation
- Database context integration
- Runtime effect tracking

**All from clean queries tested in isolation.** See how we do it → [deepstaging/effects](https://github.com/deepstaging/effects)

---

## 🎯 The Query-First Workflow

```
┌─────────────────────────────────────────┐
│ 5. Generator (Integration Tests)       │  ← Slow (seconds)
├─────────────────────────────────────────┤
│ 4. CodeFix (Transformation Tests)      │  ← Medium
├─────────────────────────────────────────┤
│ 3. Analyzer (Diagnostic Tests)         │  ← Medium  
├─────────────────────────────────────────┤
│ 2. Template (Rendering Tests)          │  ← Fast
├─────────────────────────────────────────┤
│ 1. Query (Unit Tests)                  │  ← FASTEST (milliseconds)
└─────────────────────────────────────────┘
         ↑
    Start here!
```

**Test at the fastest layer.** Catch 90% of bugs in milliseconds, not minutes.

---

## 📦 Packages

### [deepstaging/deepstaging](https://github.com/deepstaging/deepstaging)
**Core infrastructure for Roslyn tooling**

```bash
dotnet add package Deepstaging
```

Includes:
- ✅ Fluent query builders for all symbol types
- ✅ Scriban template engine integration
- ✅ Testing framework for every layer
- ✅ Type-safe projections and transformations

### [deepstaging/effects](https://github.com/deepstaging/effects)
**Complete effects system built on Deepstaging**

```bash
dotnet add package Deepstaging.Effects
```

Real-world example showing:
- ✅ Complex multi-layer generators
- ✅ Analyzers with rich diagnostics
- ✅ Code fixes that transform correctly
- ✅ Full test coverage at every layer

---

## 🏁 Get Started with Development

### Option 1: Use Deepstaging in Your Project

```bash
# Add to your analyzer/generator project
dotnet add package Deepstaging

# Add to your test project  
dotnet add package Deepstaging.Testing
```

[📚 Read the getting started guide →](https://github.com/deepstaging/deepstaging#readme)

### Option 2: Create a New Roslyn Project from Template

```bash
# Clone the workspace
git clone https://github.com/deepstaging/workspace.git
cd workspace

# Run bootstrap to set up everything
./scripts/bootstrap.sh

# Create your project
./scripts/new-roslyn-project.sh MyAwesomeTool
```

**You get:**
- ✅ Complete project structure with best practices
- ✅ Pre-configured testing setup
- ✅ Sample queries, analyzers, and generators
- ✅ Build scripts and packaging ready to go

### Option 3: Explore the Examples

**Start with the effects repository:**
```bash
git clone https://github.com/deepstaging/effects.git
cd effects
dotnet test  # See query tests run in milliseconds
```

**Key files to check out:**
- `src/Deepstaging.Effects.Queries/` - How to write queries
- `src/Deepstaging.Effects.Generators/` - How queries become generators
- `src/Deepstaging.Effects.Tests/` - Testing at every layer

---

## 🎓 Why Query-First?

**Traditional Roslyn development:**
1. Write a generator
2. Debug symbol API issues
3. Wait for slow tests
4. Fix one bug, create two more
5. Repeat until you give up or ship buggy code

**Deepstaging development:**
1. Write a query (5 minutes)
2. Test it (runs in milliseconds)
3. Fix any issues instantly
4. Use query in generator with confidence
5. Ship reliable tooling

**The result:** Faster development, fewer bugs, tooling you can trust.

---

## 🛠️ Technology

- **C# / .NET** - netstandard2.0 for analyzers/generators, net10.0 for runtime
- **Roslyn** - Microsoft.CodeAnalysis.CSharp 4.11.0+
- **Scriban** - Powerful template engine for code generation
- **TUnit** - Modern, fast testing framework

---

## 📚 Resources

- [📖 Documentation](https://github.com/deepstaging/deepstaging#readme)
- [🧪 Testing Guides](https://github.com/deepstaging/deepstaging/tree/main/src/Deepstaging.Testing)
- [🏗️ Workspace Setup](https://github.com/deepstaging/workspace#readme)
- [💬 Discussions](https://github.com/orgs/deepstaging/discussions)
- [🐛 Issues](https://github.com/deepstaging/deepstaging/issues)

---

## 📄 License

MIT - Build whatever you want with Deepstaging.

**Stop guessing. Start querying. Build reliable Roslyn tooling today.**
