# Rhinestone Reusable Workflows

**Reusable workflows for our continuous integration suite**

Workflows:

- **Forge Lint**: Lint Solidity files
- **Forge Coverage**: Generate coverage reports
- **Forge Docs**: Generate documentation
- **Forge Build**: Build the project
- **Forge Test**: Run forge tests
- **Forge Test Simulated**: Run forge tests with ERC-4337 simulation (only applicable if using ModuleKit)

## Using the workflows

In your `.yaml` file, you can use the workflows like so:

```yaml
jobs:
  lint-safe7579:
    uses: "rhinestonewtf/reusable-workflows/.github/workflows/forge-lint.yaml@main"
```

Some workflows require additional inputs, which you can provide like so:

```yaml
build-safe7579:
  uses: "rhinestonewtf/reusable-workflows/.github/workflows/forge-test.yaml@main"
  with:
    match-path: "test/**/*.sol"
```
