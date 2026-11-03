# Releasing charts

- Chart source is under `charts/`; packaged archives are under `docs/`.
- CI lints and renders charts but does not publish packages.
- `docs/index.yaml` and the root `index.yaml` are separate catalogs.
- Confirm the GitHub Pages publishing directory before updating either index.

## Package

```sh
helm package charts/web-app --destination docs
helm repo index docs --url https://helm.atakangul.com/docs --merge docs/index.yaml
```

Use `https://helm.atakangul.com` as the index URL prefix if Pages publishes `docs/` as its root.

## Verify

```sh
helm show chart docs/web-app-0.1.42.tgz
helm lint charts/web-app -f charts/web-app/values-production.yaml
```

Commit the new archive and the index served by Pages only after checking its package URLs.
