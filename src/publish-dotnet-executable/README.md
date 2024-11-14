# Publish Executable Project Action

This GitHub Action builds and publishes an executable .NET project, packaging it into a ZIP file and uploading it as an artifact.

## Inputs

- **`build_version`** (required): The build version for the project.
- **`project_name`** (required): The name of the project.
- **`project_type`** (required): The type of the project file (e.g., `csproj`).
- **`dotnet_version`** (optional): The version of the .NET SDK to use. Default is `8`.

## Example Usage

```yaml
steps:
  - name: Publish Executable Project
    uses: g4-github-actions/src/publish-executable-action@v1
    with:
      build_version: '2023.10.22.1'
      project_name: 'G4.ManifestValidator'
      project_type: 'csproj'
      dotnet_version: '8'
