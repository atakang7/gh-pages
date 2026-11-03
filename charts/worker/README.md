Part of [gh-pages](../../README.md)

# worker

Deployment for a background worker with database connection environment variables.

## Run

From the repository root:

```sh
helm upgrade --install worker charts/worker -n staging --create-namespace -f charts/worker/values-staging.yaml
```

## Test

```sh
helm lint charts/worker
helm template worker charts/worker -f charts/worker/values-staging.yaml
```

## Config

| Value | Default | Use |
| --- | --- | --- |
| `replicaCount` | `1` | Pod replicas |
| `image.repository` | `docker.io/atakan1927/worker` | Container image |
| `image.tag` | `1.0.0` | Image tag |
| `image.pullPolicy` | `IfNotPresent` | Image pull policy |
| `database.host` | `database-service` | `DB_HOST` |
| `database.port` | `27017` | `DB_PORT` |
| `database.name` | `mydatabase` | `DB_NAME` |

Staging and production overrides are in `values-staging.yaml` and `values-production.yaml`.

## Interfaces

The container receives `DB_HOST`, `DB_PORT`, and `DB_NAME`. This chart has no Service.
