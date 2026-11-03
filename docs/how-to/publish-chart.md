# Publish a chart

Set the release `version` in `charts/web-app/Chart.yaml` before packaging a new web-app revision.

From the repository root:

```sh
helm lint charts/web-app
helm package charts/web-app -d docs
helm repo index docs --url https://helm.atakangul.com --merge docs/index.yaml
git status --short docs/
```

Commit the new archive and `docs/index.yaml`. Confirm the GitHub Pages source is `docs/` before publishing, since the repository also has a root `index.yaml`.
