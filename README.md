# clusters

What runs in each Frikanalen Kubernetes cluster, as plain files. Argo CD in
each cluster syncs its own directory, so **merging here is deploying**.
See [`clusters/README.md`](clusters/README.md).

| Directory | Cluster | Synced by |
|---|---|---|
| `clusters/staging/` | kubedev (staging.frikanalen.tv) | the `staging` root Application |
| `clusters/prod/` | kubeprod (frikanalen.tv) | the `prod` root Application |

Charts live in each app's own repository (django-api, frontend, playout,
contribution-receiver, stream-monitoring, ...). Secret values live in
OpenBao, never here. The machines, each cluster's bootstrap and the root
Applications are [Frikanalen/infra](https://github.com/Frikanalen/infra)'s.

This repository used to be called `gitops` and held charts and manifests
that infra pointed at; those moved out in November 2026.
