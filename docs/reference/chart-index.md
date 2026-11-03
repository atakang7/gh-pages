# Chart index

| Path | Current contents |
| --- | --- |
| `docs/index.yaml` | database `1.0.0`, web-app `0.1.0` and `0.1.14`, worker `0.1.0`; relative archive URLs |
| `index.yaml` | Original chart versions with absolute `atakang7.github.io` archive URLs |
| `charts/web-app/Chart.yaml` | Source version `0.1.42` |
| `docs/*.tgz` | Tracked package archives, including versions absent from `docs/index.yaml` |
| `CNAME` | `helm.atakangul.com` |

Helm reads `index.yaml` from the configured repository URL and retrieves the archive named in its entry.
