# Notes for agents working in this repository

A merge to `clusters/prod` is a prod deploy; `clusters/staging` is staging.

## Alerts

A release or a deliberate restart sets off pod and replica alerts while the
pods roll. Before merging or syncing something you know will do that,
silence exactly those alerts for about as long as the rollout takes, with
the infra repo's `scripts/alert-silence` (e.g.
`scripts/alert-silence add prod 20m "django 3.12.0 (clusters#NN)" rollout:default`),
and lift it once the rollout is healthy. Anything that interrupts air or
the web stream uses the `onair` preset and needs the user's OK like any
other prod change. Tiers, presets and rules: infra `docs/alerting.md`.
