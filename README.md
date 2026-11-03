# Helm charts

This repository contains three small Helm charts for a web app, a worker, and a MongoDB database. It also stores packaged chart releases for a GitHub Pages Helm repository configured at `helm.atakangul.com`.

## Quick start

You need Helm 3.16.4. Rendering charts does not require a Kubernetes cluster.

```sh
git clone https://github.com/atakang7/gh-pages.git
cd gh-pages
helm lint charts/web-app --values charts/web-app/values-staging.yaml
helm template web-app charts/web-app --namespace staging --values charts/web-app/values-staging.yaml
```

To check all charts, run the commands in `.github/workflows/helm.yml`. CI lints and renders each chart with its default, staging, and production values.

## Configuration

Each directory under `charts/` has a `values.yaml` file and two overrides: `values-staging.yaml` and `values-production.yaml`. Values are passed to Helm with `--values`; no `.env` file is used.

- `image.repository`, `image.tag`, and `image.pullPolicy` select the container image.
- `web-app.host` names the Service. `web-app.service.port` defaults to 80 and forwards to container port 3004.
- `database.service.name` and `database.service.port` set the MongoDB Service address.
- `web-app.database.*` and `worker.database.*` configure the database connection. The default database name is `mydatabase`.

The example images use the `docker.io/atakan1927` namespace. Set image values to your own published images when deploying a different build.

## How it works

`database` creates a Deployment, Service, and ConfigMap with a MongoDB initialization script. `web-app` creates a Deployment and Service. `worker` creates a Deployment. The app and worker receive `DB_HOST`, `DB_PORT`, and `DB_NAME` from their chart values.

## Deploy and use

With a Kubernetes context configured, install the staging releases in order:

```sh
helm upgrade --install database charts/database --namespace staging --create-namespace --values charts/database/values-staging.yaml
helm upgrade --install web-app charts/web-app --namespace staging --values charts/web-app/values-staging.yaml
helm upgrade --install worker charts/worker --namespace staging --values charts/worker/values-staging.yaml
```

For production, use `--namespace production --values charts/<chart>/values-production.yaml` with each command. Helm's release namespace controls where resources are created.

The configured chart repository can be inspected with:

```sh
helm repo add atakangul https://helm.atakangul.com
helm repo update
helm search repo atakangul --versions
```

Chart source lives in `charts/`; packaged archives and an index live in `docs/`. For a release, package the chart and regenerate the index for the directory actually served by GitHub Pages:

```sh
helm package charts/web-app --destination docs
helm repo index docs --url https://helm.atakangul.com/docs --merge docs/index.yaml
```

Confirm the Pages publishing directory and chart URLs before pushing a release. There is also a root `index.yaml` from an older publishing layout.

## Limitations

The MongoDB Deployment has no persistent volume. The charts do not create an Ingress or configure health probes. Fixed chart labels and the database ConfigMap name assume one release of each chart per namespace.

The `web-app` source declares version `0.1.42`, but the newest committed package under `docs/` is `0.1.41`; the indexes are older still. Until releases are repackaged and indexed, use local chart paths rather than assuming the remote catalog matches source.
