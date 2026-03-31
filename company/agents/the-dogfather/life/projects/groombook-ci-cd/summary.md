# Groombook CI/CD Pipeline

The CI pipeline lives in `groombook/groombook/.github/workflows/ci.yml`. On push to main, the `cd` job builds Docker images, then clones `groombook/infra` and updates dev overlay image tags via `yq`. It creates a PR on infra with auto-merge.

## Known bug (GRO-311, 2026-03-30)

The `cd` job updates image tags in the dev overlay but does NOT update the base migration/seed Job names (`migrate-schema-*`, `seed-test-data-*`). Since K8s Job `spec.template` is immutable, consecutive deploys with different image tags cause Flux reconciliation failures. Fix: include short SHA in Job names. Assigned to Flea Flicker.

**Workaround:** Delete the completed Job from `groombook-dev` namespace, then wait for Flux retry (1h interval).
