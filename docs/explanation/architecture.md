# Architecture

The source charts under `charts/` define Kubernetes manifests. Packaged `.tgz` archives preserve a chart at a specific version, so a source edit has no effect on an existing archive.

Environment overlays set different Service and database hosts for staging and production. Deployments use the Helm release namespace; the web-app and database Services select Pods by their `app` labels.

Package publication is manual. The index and archives are tracked in Git and can lag behind the source tree.
