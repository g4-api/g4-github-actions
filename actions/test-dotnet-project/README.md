# dotnet-unit-test

## Overview

`dotnet-unit-test` is a GitHub Action designed to streamline the process of running unit tests for .NET projects. It not only executes your tests but also collects code coverage metrics, publishes test results, and generates comprehensive coverage reports. This action supports various project types and allows for customization through multiple input parameters.

## Features

- **Run Unit Tests**: Executes unit tests for your .NET projects.
- **Code Coverage**: Collects code coverage data using Cobertura format.
- **Test Results Publishing**: Publishes test results to GitHub Actions.
- **Artifact Uploading**: Uploads test results and coverage reports as artifacts.
- **Customizable**: Supports filters, test settings, and various project types.

## Inputs

| Input           | Description                                                      | Required | Default   |
|-----------------|------------------------------------------------------------------|----------|-----------|
| `build_version` | The build version of your project.                               | No       | `0.0.0.0` |
| `project_name`  | The name of the project to test (e.g., `MyApp`).                 | Yes      | N/A       |
| `project_type`  | The type of the project file (e.g., `csproj` or `sln`).          | Yes      | N/A       |
| `filter`        | The filter to apply to the test run (e.g., `TestCategory=Unit`). | No       | ``        |
| `test_settings` | The path to the test settings file.                              | No       | ``        |

## Outputs

This action does not produce any explicit outputs. However, it uploads artifacts such as test results and coverage summaries which can be accessed from the workflow run.

## Usage

### Prerequisites

- A .NET project with unit tests.
- GitHub Actions enabled in your repository.

### Example Workflow

Create a workflow file (e.g., `.github/workflows/dotnet-unit-test.yml`) in your repository with the following content:

```yaml
name: .NET Unit Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '8.0.x' # Specify your .NET version

      - name: Restore Dependencies
        run: dotnet restore

      - name: Build
        run: dotnet build --no-restore --configuration Release

      - name: Run Unit Tests
        uses: <repo>/dotnet-unit-test@v1
        with:
          build_version: '1.0.0'
          project_name: 'MyApp.Tests'
          project_type: 'csproj'
          filter: 'TestCategory=Unit'
          test_settings: 'path/to/testsettings.json'

      # Optionally, you can add steps to publish artifacts or further process results
```

### Input Parameters

- **`build_version`**: (Optional) Specify the build version. Defaults to `0.0.0.0` if not provided.
- **`project_name`**: (Required) The name of your test project (e.g., `MyApp.Tests`).
- **`project_type`**: (Required) The type of your project file (`csproj` or `sln`).
- **`filter`**: (Optional) Apply filters to your test run (e.g., `TestCategory=Unit`).
- **`test_settings`**: (Optional) Path to your test settings file if any.

## Artifacts

This action uploads the following artifacts:

- **Test Results**: Located under `test-results`, containing the test output XML files.
- **Code Coverage**: Located under `test-coverage`, containing Cobertura coverage reports.
- **Coverage Summary**: A Markdown summary of the coverage report appended to the GitHub Actions job summary.

You can download these artifacts from the workflow run details for further analysis.

## Acknowledgements

- Thanks to the [EnricoMi/publish-unit-test-result-action](https://github.com/EnricoMi/publish-unit-test-result-action) for publishing test results.
- Utilizes [actions/upload-artifact](https://github.com/actions/upload-artifact) for uploading artifacts.
