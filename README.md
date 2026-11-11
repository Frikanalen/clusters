# gitops

Helm charts and Argo CD manifests that the clusters consume. Nothing in this
repository deploys itself: every Argo CD Application that reads from here is
created by [Frikanalen/infra](https://github.com/Frikanalen/infra), which also
chooses the revision per environment.

| Path | What it is | Deployed by |
|---|---|---|
| `helm/mediamtx` | MediaMTX contribution/preview chart | infra `data/apps.yml` (`frikanalen_apps.mediamtx`) |
| `apps/rook-ceph/` | Rook Ceph operator, CSI drivers and cluster | infra `argocd_gitops` role, `k8s_platform_*.yml` |
| `apps/local-path-provisioner/` | Per-node SSD storage class | infra `argocd_gitops` role, `k8s_platform_*.yml` |
| `apps/stream-monitoring.yaml`, `helm/stream-monitoring` | Prod MPEG-TS stream monitor | Not yet in infra (applied by hand); to be moved |

Environment wiring, secrets and firewall configuration live in Frikanalen/infra.
