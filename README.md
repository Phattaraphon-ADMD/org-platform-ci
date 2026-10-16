# Organization Central CI/CD

Reusable GitHub Actions workflows for multiple programming languages.

## Supported languages

- Java / Maven
- Node.js / npm
- .NET
- Python / pip / pytest
- Go

## Features

- Centralized language-specific CI
- Trivy filesystem scan
- Trivy container image scan
- SonarQube code quality and security analysis (quality gate)
- Docker Buildx
- GitHub Container Registry (GHCR)
- Reusable workflows via workflow_call

## Setup

1. Create the repository `company/org-platform-ci`.
2. Add all files under `.github/workflows/`.
3. Publish a protected release tag, for example `v1`.
4. Allow application repositories to access reusable workflows.
5. Add `.github/workflows/pipeline.yml` to each application repository.
6. Call only `ci.yml` with `language`, and optionally `severity` and `image_name`.
   It runs language CI and the Trivy source scan, then builds, scans and pushes
   the image on pushes to `main` when `image_name` is set.
7. Grant `packages: write` to the calling job.
8. For SonarQube, add a `sonar-project.properties` file to each application
   repository and pass the `SONAR_TOKEN` secret (and `SONAR_HOST_URL` for
   SonarQube Server). Set `enable_sonarqube: false` to skip it, and use
   `sonarqube-args` to pass extra `-Dsonar.*` scanner arguments.
9. Configure repository rulesets and required status checks.

## Container flow

Build image locally
-> Trivy image scan
-> Login to GHCR
-> Push image

The image is not pushed when the scan fails.

## Important

- Adjust commands for each repository's build layout.
- Review and pin third-party Actions to full commit SHAs.
- Configure least-privilege permissions.
- Use OIDC for cloud deployment when supported.
- Add SAST and additional compliance checks where required.
- Protect release tags and production environments.
