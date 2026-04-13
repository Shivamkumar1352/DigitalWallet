# CI/CD Setup

This repository now has GitHub Actions workflows for:

- CI: build, test, and Docker-build every service on pull requests and pushes to `main`
- CD: publish Docker images to Docker Hub on pushes to `main`
- Deploy: optionally SSH into a server and refresh the Docker Compose stack after publishing

## Workflows

- `.github/workflows/ci.yml`
- `.github/workflows/cd.yml`

## Required GitHub Secrets

Set these in `GitHub repository -> Settings -> Secrets and variables -> Actions`.

### For image publishing

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`

If these are not configured yet, the CD workflow file remains present but the publish job is skipped.

The CD workflow pushes:

- `DOCKERHUB_USERNAME/api-gateway:latest`
- `DOCKERHUB_USERNAME/auth-service:latest`
- `DOCKERHUB_USERNAME/eureka-server:latest`
- `DOCKERHUB_USERNAME/notification-service:latest`
- `DOCKERHUB_USERNAME/reward-service:latest`
- `DOCKERHUB_USERNAME/user-service:latest`
- `DOCKERHUB_USERNAME/wallet-service:latest`

It also pushes a short-SHA tag for each image.

### For automatic deployment

These are only needed if you want the deploy job to run.

- `DEPLOY_HOST`
- `DEPLOY_USER`
- `DEPLOY_SSH_KEY`
- `DEPLOY_PATH`
- `DEPLOY_PORT` (optional, defaults to `22`)

## Server Preparation

On the target server:

1. Install Docker and Docker Compose.
2. Clone this repository into the directory used for `DEPLOY_PATH`.
3. Create the root `.env` file with the runtime secrets your services need.
4. Log in once with `docker login` so the server can pull your private images if needed.

When the deploy job runs, it executes:

```bash
docker compose pull
docker compose up -d --remove-orphans --no-build
```

## Recommended Branch Flow

- Open a pull request to trigger CI.
- Merge into `main` to trigger image publishing.
- If deploy secrets are configured, the same `main` push will deploy automatically.

## Notes

- The current `docker-compose.yml` already points at Docker Hub images, so the CD workflow matches the existing container naming pattern.
- If you want stricter quality gates, the next step is adding service tests and optionally a SonarQube workflow using `scripts/run-sonar.sh`.
