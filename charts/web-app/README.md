Part of [gh-pages](../../README.md)

# web-app

Deployment and ClusterIP Service for a container listening on port `3004`.

## Run

From the repository root:

```sh
helm upgrade --install web-app charts/web-app -n staging --create-namespace -f charts/web-app/values-staging.yaml
```

## Test

```sh
helm lint charts/web-app
helm template web-app charts/web-app -f charts/web-app/values-staging.yaml
```

## Config

| Value | Default | Use |
| --- | --- | --- |
| `replicaCount` | `1` | Pod replicas |
| `image.repository` | `docker.io/atakan1927/web-app` | Container image |
| `image.tag` | `1.0.0` | Image tag |
| `image.pullPolicy` | `IfNotPresent` | Image pull policy |
| `host` | `web-app-service` | Service name |
| `service.type` | `ClusterIP` | Service type |
| `service.port` | `3004` | Service port |
| `database.host` | `database-service` | `DB_HOST` |
| `database.port` | `27017` | `DB_PORT` |
| `database.name` | `mydatabase` | `DB_NAME` |

Staging and production overrides are in `values-staging.yaml` and `values-production.yaml`.

## Interfaces

The Service forwards TCP port `3004` to the Pod on port `3004`. The container receives `DB_HOST`, `DB_PORT`, and `DB_NAME`.
