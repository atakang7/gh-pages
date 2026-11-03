# Helm Charts

This repository hosts a curated set of Helm charts for deploying applications to Kubernetes clusters. It serves as a Helm chart repository via GitHub Pages at **[helm.atakangul.com](https://helm.atakangul.com)**.

## Available Charts

| Chart | Description | Latest Version |
|-------|-------------|----------------|
| `web-app` | Web application deployment chart with staging/production value overlays | `0.1.42` |
| `database` | Database deployment chart | `1.0.0` |
| `worker` | Background worker application chart | `0.1.0` |

Each chart supports environment-specific values via `values-staging.yaml` and `values-production.yaml` overlays.

## Usage

### Add the repository

```bash
helm repo add atakangul https://helm.atakangul.com
helm repo update
```

### Search for available charts

```bash
helm search repo atakangul
```

### Install a chart

```bash
# Install the web-app chart with staging values
helm install my-web-app atakangul/web-app -f values-staging.yaml

# Install the database chart
helm install my-db atakangul/database
```

### Deploy a specific environment

```bash
helm install my-web-app atakangul/web-app \
  --values values-production.yaml \
  --namespace production
```

## Chart Development

Charts are maintained under `charts/` with packaged releases published to `docs/`. The `docs/` directory contains the Helm repository index and all packaged `.tgz` releases, served automatically via GitHub Pages.

### Package and update the index

```bash
# Package a chart
helm package charts/web-app -d docs/

# Regenerate the repository index
helm repo index docs/ --url https://helm.atakangul.com
```

### Lint a chart

```bash
helm lint ./charts/web-app
```

## Repository Structure

```
.
├── charts/          # Chart source files
│   ├── database/    # Database deployment chart
│   ├── web-app/     # Web application chart
│   └── worker/      # Worker application chart
├── docs/            # GitHub Pages content (Helm repo index + packaged charts)
├── CNAME            # Custom domain configuration
└── README.md
```
