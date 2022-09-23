# Actions build and publish packages to Planday's ProGet feed

This repository contains a Github action which purpose is to build and publish a package (`NuGet` or `npm`) to Planday's ProGet feed using a `Dockerfile`.

## Usage

### Inputs

| Name | Type | Required | Description |
| :---: | :---: | :---: |  --- |
| `image_name` | `string` | `true` | Name of the Docker image to build. (ex: 'myapp') |
| `version_suffix` | `string` | `false` | Package version suffix. Set this to empty to move the package out of prerelease. Default: `beta` |
| `sonarqube_project_name` | `string` | `false` | Name of the project in SonarQube |
| `sonarqube_version` | `string` | `false` | Version of the SonarQube project |
| `registrydev` | `string` | `true` | Azure dev Containers Registry to pull base images from |
| `dockerargs` | `string` | `false` | Additional build arguments for docker, provided as a multiline string |

### Secrets
| Name | Required | Description |
| :---: | :---: | :---: |
| `acr_username` | `true` | Service Principal ID for Azure ACR |
| `acr_password` | `true` | Service Principal password for Azure ACR |
| `packages_api_key` | `true` | API key of the ProGet feed server |
| `packages_feed_url` | `true` | URL of the ProGet feed server |
| `sonarqube_token` | `false` | SonarQube server token |
| `sonarqube_server_url` | `false` | URL of the SonarQube server |

## Examples
### Nuget

Use this workflow in your pipeline:

```yaml
jobs:
  build-and-publish:
    uses: planday-corp/actions-publish-packages/.github/workflows/publish-nuget-packages.yml@v1
    with:
      image_name: "myproject"
      registrydev: "registrydev.azurecr.io"
      version_suffix: "rc"
      sonarqube_project_name: "myproject"
      sonarqube_version: "3.2.1"
      dockerargs: |
        BUILD_VAR1=myvar1
        BUILD_VAR2=myvar2
    secrets:
      packages_feed_url: ${{ secrets.PACKAGES_FEED_URL }}
      acr_username: ${{secrets.ACR_USERNAME}}
      acr_password: ${{secrets.ACR_PASSWORD}}
      sonarqube_token: ${{secrets.SONARQUBE_TOKEN}}
      packages_api_key: ${{secrets.PACKAGES_DEFAULT_API_KEY}}
```

### NPM *(WIP)*
