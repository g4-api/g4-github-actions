# dotnet-unit-test

## Overview

`dotnet-unit-test` is a custom GitHub Action designed to streamline the process of running unit tests for .NET projects. It automates the setup of the .NET environment, executes your unit tests, collects code coverage metrics, and uploads the results as artifacts. Additionally, it generates a comprehensive code coverage summary to help you monitor the quality of your codebase.

## Features

- **Automated .NET Setup**: Configures the specified .NET Core version.
- **Flexible Project Configuration**: Supports both `.csproj` and `.sln` project types.
- **Customizable Test Runs**: Apply filters and use custom test settings.
- **Code Coverage Collection**: Gathers and uploads code coverage results.
- **Artifact Management**: Stores test results and coverage reports as artifacts.
- **Detailed Coverage Summary**: Generates a Markdown summary of code coverage metrics.

## Inputs

| Input                    | Description                                                      | Required | Default         |
| -------------------------|------------------------------------------------------------------|----------|-----------------|
| `build-version`          | The build version of your project.                               | No       | `0.0.0.0`       |
| `dotnet-version`         | The .NET Core version to use.                                    | No       | `8.0.x`         |
| `project-name`           | **Name of the project** (without extension).                     | Yes      | N/A             |
| `project-type`           | **Type of the project file** (e.g., `csproj` or `sln`).          | Yes      | N/A             |
| `filter`                 | The filter to apply to the test run (e.g., `TestCategory=Unit`). | No       | `""`            |
| `test-settings`          | The path to the test settings file.                              | No       | `""`            |
| `results-artifact-name`  | The name of the artifact to store the test results.              | No       | `test-results`  |
| `coverage-artifact-name` | The name of the artifact to store the code coverage results.     | No       | `test-coverage` |

## Usage

To use the `dotnet-unit-test` action in your workflow, include the following step in your GitHub Actions workflow file:

```yaml
jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Run .NET Unit Tests
        uses: your-repo/dotnet-unit-test@v1
        with:
          build-version: '1.2.3.4'                     # Optional: Specify your build version
          dotnet-version: '8.0.x'                      # Optional: Specify .NET Core version
          project-name: 'MyProject'                    # Required: Name of your project
          project-type: 'csproj'                       # Required: Project file type (`csproj` or `sln`)
          filter: 'TestCategory=Unit'                  # Optional: Test filter expression
          test-settings: 'test.runsettings'            # Optional: Path to test settings file
          results-artifact-name: 'unit-test-results'   # Optional: Custom name for test results artifact
          coverage-artifact-name: 'unit-test-coverage' # Optional: Custom name for coverage results artifact
```

### Example Workflow

Here's a complete example of a GitHub Actions workflow utilizing the `dotnet-unit-test` action:

```yaml
name: .NET Unit Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Run .NET Unit Tests
        uses: your-repo/dotnet-unit-test@v1
        with:
          build-version: '1.0.0'
          dotnet-version: '8.0.x'
          project-name: 'MyApp'
          project-type: 'sln'
          filter: 'TestCategory=Unit'
          test-settings: 'tests/test.runsettings'
```

## Action Steps

1. **Setup .NET**: Configures the specified .NET Core version using `actions/setup-dotnet@v4`.
2. **Set Working Directory**: Identifies the project file and sets the working directory accordingly.
3. **Unit Tests and Code Coverage**: Executes the `dotnet test` command with optional filters and settings, and collects code coverage data.
4. **Upload Unit Tests Results**: Uploads the test results as an artifact.
5. **Copy Coverage File**: Locates and copies the coverage report to the artifact staging directory.
6. **Upload Code Coverage Results**: Uploads the code coverage report as an artifact.
7. **Generate Code Coverage Summary**: Creates a Markdown summary of the code coverage metrics.
8. **Upload Code Coverage Summary**: Uploads the coverage summary as an artifact.
9. **Collect Job Summary**: Appends the coverage summary to the GitHub Actions job summary for easy viewing.

## Outputs

This action does not produce any direct outputs. However, it uploads the following artifacts:

- **Test Results**: Contains the XML test results.
- **Code Coverage Results**: Contains the code coverage report in Cobertura XML format.
- **Coverage Summary**: A Markdown file summarizing the code coverage metrics.

## Requirements

- **GitHub Repository**: Ensure that your repository is set up with GitHub Actions.
- **.NET Project**: The action is designed for .NET Core projects using `.csproj` or `.sln` files.
- **Test Settings File** *(optional)*: If you need custom test settings, provide a `.runsettings` file.
