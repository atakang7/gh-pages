# Helm charts

Helm charts for `web-app`, `worker`, and `database`, with packaged releases under `docs/`.

```text
+---------+            +--------+
| web-app |            | worker |
+---------+            +--------+
     |                     |
  TCP 27017            TCP 27017
     v                     v
+-----------------------------+
|          database           |
+-----------------------------+
```

CI (push or pull request):

```text
+---------+
| charts/ |
+---------+
     |
  changes
     v
+-----------------------------+
| .github/workflows/helm.yml  |
+-----------------------------+
     |                  |
  chart YAML        chart YAML
     v                  v
+-----------+      +---------------+
| helm lint |      | helm template |
+-----------+      +---------------+
```

## Run

Helm 3.16.4 is used in CI; installation needs a configured Kubernetes context.

```sh
git clone https://github.com/atakang7/gh-pages.git
cd gh-pages
helm lint charts/web-app -f charts/web-app/values-staging.yaml
helm template web-app charts/web-app -n staging -f charts/web-app/values-staging.yaml
helm upgrade --install database charts/database -n staging --create-namespace -f charts/database/values-staging.yaml
helm upgrade --install web-app charts/web-app -n staging -f charts/web-app/values-staging.yaml
helm upgrade --install worker charts/worker -n staging -f charts/worker/values-staging.yaml
```

For production, replace `staging` with `production` in the namespace and values filenames.

## Config

| Value | Default | What |
| --- | --- | --- |
| `*.replicaCount` | `1` | Replicas |
| `*.image.repository` | `docker.io/atakan1927/<chart>` | Image |
| `*.image.tag` | `1.0.0` | Image tag; web-app overlays use `0.1.42` |
| `*.image.pullPolicy` | `IfNotPresent` | Pull policy |
| `web-app.host` | `web-app-service` | Service name |
| `web-app.service.port` | `80` | Service port; container uses `3004` |
| `database.service.name` | `database-service` | MongoDB Service name |
| `database.service.port` | `27017` | MongoDB port |
| `{web-app,worker}.database.*` | `database-service`, `27017`, `mydatabase` | `DB_HOST`, `DB_PORT`, `DB_NAME` |

Environment overrides live in `charts/<chart>/values-staging.yaml` and `values-production.yaml`.

## Limits

- MongoDB has no persistent volume.
- Charts have no Ingress or health probes.
- Fixed labels and the database ConfigMap name can conflict across releases in one namespace.
- The newest packaged web-app chart under `docs/` is `0.1.41`; source is `0.1.42`.
- Publishing is manual; see [release steps](docs/RELEASING.md).
