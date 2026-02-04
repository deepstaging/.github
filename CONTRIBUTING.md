# Contributing to Deepstaging

Thanks for your interest in contributing! Here's how to get started.

## Getting Started

1. Fork the repository
2. Clone your fork locally
3. Create a branch for your changes

```bash
git checkout -b my-feature
```

## Development Setup

Both repositories use .NET 9 and the slnx solution format:

```bash
# Clone and build
git clone https://github.com/deepstaging/<repo>.git
cd <repo>
dotnet build

# Run tests
dotnet test
```

## Making Changes

### Code Style

- Follow existing code conventions in the repository
- Use meaningful names for types, methods, and variables
- Keep methods focused and small
- Add XML documentation for public APIs

### Commits

- Write clear, concise commit messages
- Use present tense ("Add feature" not "Added feature")
- Reference issues when applicable (`Fixes #123`)

### Testing

- Add tests for new functionality
- Ensure existing tests pass before submitting
- For Roslyn code, test at the appropriate layer (queries, templates, analyzers, generators)

## Submitting Changes

1. Push your branch to your fork
2. Open a pull request against `main`
3. Fill out the PR template
4. Wait for review

## Pull Request Guidelines

- Keep PRs focused on a single change
- Update documentation if needed
- Add tests for new features
- Ensure CI passes

## Reporting Issues

- Check existing issues first
- Use issue templates when available
- Provide reproduction steps for bugs
- Be specific about expected vs actual behavior

## License

By contributing, you agree that your contributions will be licensed under RPL-1.5.

## Questions?

Open a [discussion](https://github.com/orgs/deepstaging/discussions) if you have questions.
