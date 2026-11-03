# gh-pages

Helm chart sources and packaged releases for Kubernetes operators using `helm.atakangul.com`.

```text
+-----------------+   helm package   +------------+
| charts/database | ---------------> | docs/*.tgz |
| charts/web-app  |                  +------------+
| charts/worker   |                         |
+-----------------+                         v indexed by
                                      +-----------------+
                                      | docs/index.yaml |
                                      +-----------------+
```

## Quick start

Use Helm `3.16.4` for the same checks as CI.

```sh
helm repo add atakangul https://helm.atakangul.com
helm repo update
helm search repo atakangul
helm lint charts/web-app
helm template web-app charts/web-app -f charts/web-app/values-staging.yaml
```

## Layout

| Path | What it is |
| --- | --- |
| [`charts/database`](charts/database/README.md) | Database Deployment, ConfigMap and Service |
| [`charts/web-app`](charts/web-app/README.md) | Web Deployment and Service |
| [`charts/worker`](charts/worker/README.md) | Worker Deployment |
| [`docs/`](docs/README.md) | Chart archives, repository index and maintainer docs |

## Docs

[Publish a chart](docs/how-to/publish-chart.md) · [Chart index](docs/reference/chart-index.md) · [Architecture](docs/explanation/architecture.md)

## Limits

- The repository index does not list every tracked archive. See [chart index](docs/reference/chart-index.md).
- The charts have no Ingress or persistent storage templates.
