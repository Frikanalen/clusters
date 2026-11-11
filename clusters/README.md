# clusters

What runs in each Frikanalen Kubernetes cluster, as plain files. Argo CD in
each cluster syncs its own directory, so **merging here is deploying**:

| Directory | Cluster | Synced by |
|---|---|---|
| `staging/` | kubedev (staging.frikanalen.tv) | the `staging` root Application |
| `prod/` | kubeprod (frikanalen.tv) | the `prod` root Application |

Each environment directory holds:

- `apps/`: one Argo CD Application per app, with its version
  (`targetRevision` and image tag) and its values written out in full. On
  staging the versions are `main` and Argo CD Image Updater follows the
  digest; on prod they are release tags, and charts from other repos
  (contribution-receiver, stream-monitoring) are pinned to a commit.
- `platform/`: cluster services Argo CD runs, such as rook-ceph and
  local-path-provisioner. (Traefik, MetalLB, cert-manager and monitoring
  are still installed by infra with Helm, and will move here later.)
- `secrets/`: one ExternalSecret per Kubernetes Secret. The values live in
  OpenBao on bao1 at `kv/<env>/<namespace>/<name>`; External Secrets Operator
  copies them in. No secret value is ever committed here.

The root Application that points Argo CD at a directory, Argo CD itself,
External Secrets Operator and OpenBao are set up by
[Frikanalen/infra](https://github.com/Frikanalen/infra), which owns the
machines and each cluster's bootstrap. Charts live in each app's own
repository.

## Releasing to an environment

Change `targetRevision` and the image tag in `<env>/apps/<app>.yaml`, open a
PR, merge. Argo CD rolls it out and corrects any drift on its own.

On prod, playout's `checksum/live-credential` and ingest's `secretChecksum`
still carry the values infra last computed, so that the handover restarted
nothing. Reloader now restarts a workload when its Secret changes, so both
can go the next time playout or ingest is released.
