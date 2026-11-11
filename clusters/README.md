# clusters

What runs in each Frikanalen Kubernetes cluster, as plain files. Argo CD in
each cluster syncs its own directory, so **merging here is deploying**:

| Directory | Cluster | Synced by |
|---|---|---|
| `staging/` | kubedev (staging.frikanalen.tv) | the `staging` root Application |
| `prod/` | kubeprod (frikanalen.tv) | not yet; prod is still deployed from infra |

Each environment directory holds:

- `apps/`: one Argo CD Application per app, with its version
  (`targetRevision` and image tag) and its values written out in full. On
  staging the versions are `main` and Argo CD Image Updater follows the
  digest; on prod they will be release tags.
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
