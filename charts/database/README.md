Part of [gh-pages](../../README.md)

# database

MongoDB Deployment, initialization ConfigMap, and TCP Service.

## Run

From the repository root:

```sh
helm upgrade --install database charts/database -n staging --create-namespace -f charts/database/values-staging.yaml
```

## Test

```sh
helm lint charts/database
helm template database charts/database -f charts/database/values-staging.yaml
```

## Config

| Value | Default | Use |
| --- | --- | --- |
| `replicaCount` | `1` | Pod replicas |
| `image.repository` | `docker.io/atakan1927/database` | Container image |
| `image.tag` | `1.0.0` | Image tag |
| `image.pullPolicy` | `IfNotPresent` | Image pull policy |
| `service.name` | `database-service` | Deployment and Service name |
| `service.port` | `27017` | Service port |

Staging and production overrides are in `values-staging.yaml` and `values-production.yaml`.

## Interfaces

The Service forwards TCP port `27017` to the database container. The ConfigMap mounts `init.js` at `/docker-entrypoint-initdb.d` and creates `mydatabase.data`.
