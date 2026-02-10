# Mirror Image Releaser

Centralized repo for mirroring [Docker Hardened Images (DHI)](https://hub.docker.com/hardened-images) to `ghcr.io/nirmata`.

DHI images are CIS-compliant, zero-CVE, multi-arch (`linux/amd64` + `linux/arm64`), and free for base variants.

## Images

| Image | Workflow | Source | Target |
|-------|----------|--------|--------|
| **OTel Collector** | `mirror-otel-collector.yml` | `dhi.io/opentelemetry-collector:<ver>-contrib` | `ghcr.io/nirmata/otel-collector:nirmata-<ver>` |

## Usage

1. Go to **Actions** → select the workflow for the image you want to mirror
2. Click **Run workflow**
3. Enter the version and click **Run**

### Example: Mirror OTel Collector

```
Actions → Mirror OTel Collector (DHI) → Run workflow
  otel_version: 0.145.0
  variant: contrib
  push: true
```

Result: `ghcr.io/nirmata/otel-collector:nirmata-0.145.0`

## Required Secrets

| Secret | Description |
|--------|-------------|
| `DHI_USERNAME` | Docker Hub username (for `docker login dhi.io`) |
| `DHI_PASSWORD` | Docker Hub password or access token |

`GITHUB_TOKEN` is automatically available for pushing to GHCR.

