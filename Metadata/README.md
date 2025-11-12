# Metadata

This folder contains metadata configuration files used for package management and build definitions.

## Contents

| File               | Description                                                   |
|---------------------|---------------------------------------------------------------|
| `nuget.config`     | NuGet configuration file specifying package sources. |
| `packages.config`  | List of NuGet package dependencies required for this project, update version number based on production environment. |

## Notes

- Keep both files under source control.
- Always review changes to these files carefully.
- Credentials should **never** be stored in the repository.
