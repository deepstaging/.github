# Deepstaging

**Query-first infrastructure for building Roslyn source generators, analyzers, and code transformations.**

## Philosophy

> Get the queries right, and everything else follows naturally.

Deepstaging is built on the principle that **querying code is the foundation** of great tooling. When you can accurately extract and project symbol data from your codebase, building source generators, analyzers, and code fixes becomes straightforward and reliable.

## What We Build

### 🏗️ [deepstaging/deepstaging](https://github.com/deepstaging/deepstaging)
Core Roslyn tooling infrastructure that provides:
- **Fluent Query Builders** - Extract and filter Roslyn symbols with a clean, composable API
- **Template-Based Generation** - Scriban-powered source generation with type-safe models
- **Testing Framework** - Fast, focused testing at every layer of your tooling stack
- **Project Templates** - Scaffold new Roslyn projects with best practices baked in

Perfect for building your own source generators, analyzers, and code transformations.

### ⚡ [deepstaging/effects](https://github.com/deepstaging/effects)
An effects system built on Deepstaging infrastructure, demonstrating real-world usage of the core tooling. Shows how to:
- Build complex source generators from queries
- Create analyzers that understand your domain
- Provide code fixes that transform code correctly
- Test everything at the query layer first

## The Query-First Approach

```
┌────────────────────────────────────────┐
│ Generator Tests (Integration)          │
├────────────────────────────────────────┤
│ CodeFix Tests (Transformations)        │
├────────────────────────────────────────┤
│ Analyzer Tests (Diagnostics)           │
├────────────────────────────────────────┤
│ Template Tests (Rendering)             │
├────────────────────────────────────────┤
│ Query Tests (FOUNDATION)               │ ← Start here
└────────────────────────────────────────┘
```

1. **Start with queries** - Extract symbol data correctly
2. **Test thoroughly** - Fast, focused tests catch issues early  
3. **Build layers** - Templates, analyzers, and fixes use proven queries
4. **Fast feedback** - Most testing at the fastest layer (milliseconds)

## Technology

- **C# / .NET** - netstandard2.0 for broad compatibility
- **Roslyn** - Microsoft.CodeAnalysis.CSharp 4.11.0+
- **Scriban** - Powerful template engine for code generation
- **Modern .NET** - Testing framework requires .NET 10.0+

## Get Started

### Use Deepstaging in your project:
```bash
dotnet add package Deepstaging.Roslyn
dotnet add package Deepstaging.Generators
```

### Create a new Roslyn project:
```bash
# Clone the workspace
git clone https://github.com/deepstaging/deepstaging-workspace
cd deepstaging-workspace/scripts

# Generate a new project
./new-roslyn-project.sh MyAwesomeTool
```

### Explore the examples:
- Check out [deepstaging/effects](https://github.com/deepstaging/effects) for a complete real-world implementation
- Browse the samples in the main repo

## Why Deepstaging?

**Traditional approach:** Jump straight to writing source generators and hope your symbol queries work.

**Deepstaging approach:** Build and test queries first, then compose them into generators with confidence.

The result? Faster development, fewer bugs, and tooling you can trust.

---

## Resources

- 📚 [Documentation](https://github.com/deepstaging/deepstaging#readme)
- 🧪 [Testing Guides](https://github.com/deepstaging/deepstaging/tree/main/packages/Deepstaging.Testing)
- 💬 [Discussions](https://github.com/orgs/deepstaging/discussions)
- 🐛 [Issues](https://github.com/deepstaging/deepstaging/issues)

## License

MIT - Build whatever you want with Deepstaging.
