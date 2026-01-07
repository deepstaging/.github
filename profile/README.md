# Deepstaging

**Modern infrastructure for building Roslyn source generators, analyzers, and code transformations.**

A complete toolchain for building production-ready Roslyn tooling—from templates to testing to deployment.

---

## 🎯 What is Deepstaging?

Deepstaging is a **full-stack development platform** for Roslyn-based tools. We provide:

1. **Core Libraries** - Fluent query builders and template-based generation
2. **Testing Framework** - Fast, layered testing for queries, generators, analyzers, and code fixes
3. **Project Templates** - Production-ready scaffolding with best practices built-in
4. **Development Workspace** - Multi-repository orchestration and automation

**Build complete Roslyn tooling in minutes, not weeks.**

---

## 📦 Repositories

### [deepstaging/roslyn](https://github.com/deepstaging/roslyn)
**Core Roslyn infrastructure and testing framework**

Three interconnected packages:
- **Deepstaging.Roslyn** - Fluent query builders for finding and projecting symbols
- **Deepstaging.Roslyn.Generators** - Template-based source generation with Scriban
- **Deepstaging.Roslyn.Testing** - Fast testing infrastructure for every layer

```bash
dotnet add package Deepstaging.Roslyn
dotnet add package Deepstaging.Roslyn.Testing --version prerelease
```

### [deepstaging/templates](https://github.com/deepstaging/templates)
**Project templates for scaffolding new Roslyn tools**

Installable .NET templates that generate production-ready projects:
- **deepstaging-roslyn** - Complete Roslyn tooling suite (analyzer, generator, code fix, tests)
- Pre-configured with best practices
- Full test coverage from day one
- Ready to publish to NuGet

```bash
dotnet new install Deepstaging.Templates
dotnet new deepstaging-roslyn -n MyAwesomeTool
```

### [deepstaging/workspace](https://github.com/deepstaging/workspace)
**Multi-repository control plane for development**

TypeScript-based automation workspace:
- Create repositories from templates
- Orchestrate builds across projects
- Manage local NuGet feeds
- AI-powered commit workflows
- Batch package updates

```bash
git clone https://github.com/deepstaging/workspace.git
cd workspace && ./scripts/bootstrap/index.ts
```

---

## 🚀 Quick Start

### Option 1: Add to Existing Project

```bash
# Add core library to your analyzer/generator project
dotnet add package Deepstaging.Roslyn

# Add testing framework
dotnet add package Deepstaging.Roslyn.Testing
```

### Option 2: Start from Template

```bash
# Install templates
dotnet new install Deepstaging.Templates

# Create new project
dotnet new deepstaging-roslyn -n MyTool
cd MyTool

# Build and test
dotnet build
dotnet test
```

### Option 3: Use the Workspace

```bash
# Clone and bootstrap
git clone https://github.com/deepstaging/workspace.git
cd workspace
./scripts/bootstrap/index.ts

# Create repository from template
workspace-repository-create
```

---

## ✨ Key Features

### 🔍 Fluent Query Builders

Write readable code that finds symbols without loops or null checks:

```csharp
var methods = compilation
    .QueryTypes()
    .ThatArePublic()
    .ThatAreClasses()
    .WithAttribute("MyAttribute")
    .SelectMany(t => t.GetMembers()
        .OfType<IMethodSymbol>()
        .Where(m => m.ReturnsVoid));
```

### 🎨 Template-Based Generation

Generate code with Scriban templates, not string builders:

```csharp
context.AddFromTemplate(
    Named("ApiClient.scriban-cs"),
    "GeneratedClient.g.cs",
    new { Controllers = controllers });
```

### ⚡ Fast Testing

Test your queries in milliseconds, not seconds:

```csharp
[Test]
public async Task FindsEmailValidatorMethods()
{
    var result = await Query.FindValidators(compilation);
    Assert.That(result.Count(), Is.EqualTo(3));
}
```

### 🎯 Layered Architecture

Build tooling in layers, testing each independently:
- **Queries** - Find and project symbols (test in ms)
- **Templates** - Render code (test in ms)
- **Analyzers** - Detect patterns (test fast)
- **CodeFixes** - Transform code (test medium)
- **Generators** - Generate code (test slower)

---

## 🛠️ Technology Stack

- **C# / .NET** - netstandard2.0 for analyzers/generators, modern .NET for runtime
- **Roslyn** - Microsoft.CodeAnalysis.CSharp 4.14.0+
- **Scriban** - Template engine for code generation
- **TUnit** - Modern, fast testing framework

---

## 📚 Documentation

- [📖 Roslyn Library Docs](https://github.com/deepstaging/roslyn#readme)
- [🧪 Testing Framework Guide](https://github.com/deepstaging/roslyn/tree/main/src/Deepstaging.Roslyn.Testing)
- [📦 Template Documentation](https://github.com/deepstaging/templates#readme)
- [🏗️ Workspace Setup](https://github.com/deepstaging/workspace#readme)
- [💬 Discussions](https://github.com/orgs/deepstaging/discussions)

---

## 📄 License

RPL-1.5 (Reciprocal Public License) - Share improvements back to the community.

All Deepstaging repositories are licensed under RPL-1.5. See individual repository LICENSE files for details.

---

**Ready to build Roslyn tooling the modern way?**

Start with the [roslyn repository](https://github.com/deepstaging/roslyn) or create a project from [templates](https://github.com/deepstaging/templates).
