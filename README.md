# Mirror Image Releaser

Centralized repo for mirroring [Docker Hardened Images (DHI)](https://hub.docker.com/hardened-images) to `ghcr.io/nirmata`.

DHI images are CIS-compliant, zero-CVE, multi-arch (`linux/amd64` + `linux/arm64`), and free for base variants.

## Images

| Image | Workflow | Source | Target | Version Format |
|-------|----------|--------|--------|----------------|
| **OTel Collector** | `mirror-otel-collector.yml` | `dhi.io/opentelemetry-collector:<ver>-contrib` | `ghcr.io/nirmata/otel-collector:nirmata-<ver>` | Patch (e.g., `0.145.0`) |
| **kubectl** | `mirror-kubectl.yml` | `dhi.io/kubectl:<ver>` | `ghcr.io/nirmata/kubectl:<ver>` | Minor (e.g., `1.33`) |
| **etcd** | `mirror-etcd.yml` | `dhi.io/etcd:<ver>` | `ghcr.io/nirmata/etcd:<ver>` | Minor (e.g., `3.5`) |
| **PostgreSQL** | `mirror-postgres.yml` | `dhi.io/postgres:<ver>` | `ghcr.io/nirmata/postgres:<ver>` | Minor (e.g., `18.2`) |
| **CloudNativePG** | `mirror-cloudnative-pg.yml` | `dhi.io/cloudnative-pg:<ver>-debian13` | `ghcr.io/nirmata/cloudnative-pg:<ver>-dhi<n>` | Patch (e.g., `1.30.0`) |
| **CNPG Barman Cloud Plugin** | `mirror-barman-cloud-plugin.yml` | `dhi.io/cloudnative-pg-plugin-barman-cloud:<ver>-debian13` | `ghcr.io/nirmata/plugin-barman-cloud:<ver>-dhi<n>` | Patch (e.g., `0.13.0`) |
| **CNPG Barman Cloud Plugin Sidecar** | `mirror-barman-cloud-sidecar.yml` | `dhi.io/cloudnative-pg-plugin-barman-cloud-sidecar:<ver>-debian13` | `ghcr.io/nirmata/plugin-barman-cloud-sidecar:<ver>-dhi<n>` | Patch (e.g., `0.13.0`) |

## Usage

1. Go to **Actions** → select the workflow for the image you want to mirror
2. Click **Run workflow**
3. Enter the version and click **Run**

### Examples

**OTel Collector:**
```
Actions → Mirror OTel Collector (DHI) → Run workflow
  otel_version: 0.145.0
  variant: contrib
  push: true
→ ghcr.io/nirmata/otel-collector:nirmata-0.145.0
```

**kubectl:**
```
Actions → Mirror kubectl (DHI) → Run workflow
  kubectl_version: 1.33
  push: true
→ ghcr.io/nirmata/kubectl:1.33
```

**etcd:**
```
Actions → Mirror etcd (DHI) → Run workflow
  etcd_version: 3.5
  push: true
→ ghcr.io/nirmata/etcd:3.5
```

**CloudNativePG:**
```
Actions → Mirror CloudNativePG (DHI) → Run workflow
  source_version: 1.30.0-debian13
  target_tag: 1.30.0-dhi1
  push: true
→ ghcr.io/nirmata/cloudnative-pg:1.30.0-dhi1
```

**CNPG Barman Cloud Plugin:**
```
Actions → Mirror CloudNativePG Barman Cloud Plugin (DHI) → Run workflow
  source_version: 0.13.0-debian13
  target_tag: 0.13.0-dhi1
  push: true
→ ghcr.io/nirmata/plugin-barman-cloud:0.13.0-dhi1
```

**CNPG Barman Cloud Plugin Sidecar:**
```
Actions → Mirror CloudNativePG Barman Cloud Plugin Sidecar (DHI) → Run workflow
  source_version: 0.13.0-debian13
  target_tag: 0.13.0-dhi1
  push: true
→ ghcr.io/nirmata/plugin-barman-cloud-sidecar:0.13.0-dhi1
```

## What this replaces

| Image | Old Process | New Process |
|-------|-------------|-------------|
| **OTel Collector** | Build from source with `ocb`, custom Dockerfile (`otel-collector-releaser`) | Direct mirror of DHI pre-built image |
| **kubectl** | Download kubectl binary into alpine, custom Dockerfile (`nirmata/kubectl`) | Direct mirror of DHI pre-built image |
| **etcd** | Pull `quay.io/coreos/etcd`, patch CVEs with Copa, manual buildkit (`nirmata/etcd`) | Direct mirror of DHI pre-built image |

## Important Notes

- **DHI version tags**: kubectl and etcd use **minor version** tags (e.g., `1.33`, `3.5`), not patch versions. OTel Collector uses patch versions (e.g., `0.145.0`).
- **DHI images are minimal**: They may not include a shell. If you need shell access for debugging, use the `-dev` variant (if available).
- **No custom build step**: We use `docker buildx imagetools create` to directly mirror the manifest — no Dockerfile, no rebuilding layers.

## Required Secrets

| Secret | Description |
|--------|-------------|
| `DHI_USERNAME` | Docker Hub username (for `docker login dhi.io`) |
| `DHI_PASSWORD` | Docker Hub password or access token |

`GITHUB_TOKEN` is automatically available for pushing to GHCR.
