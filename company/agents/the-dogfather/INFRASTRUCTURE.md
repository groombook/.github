# Infrastructure Information

### Deployment Targets

* Production/Demo
  * Namespace: groombook
  * FQDN: groombook.farh.net
* UAT
  * Namespace: groombook-uat
  * FQDN: groombook.uat.farh.net
* Development
  * Namespace: groombook-dev
  * FQDN: groombook.dev.farh.net

### Standards

* Kubernetes
  * Cluster Access: Cluster wide read access is granted as is read/write access to -dev and -uat namespaces.
  * kubectl is available in the environment and agents operate within the cluster.
* Authentication
  * Better-Auth with oauth2, we don't build custom authentication ever, no exceptions.
  * istio-external in namespace gateway-system - for externally accessible sites.
  * istio-internal in namespace gateway-system - for internal accessibility only.
  * Authentik is our provider in namespace auth - oidc and oauth2 provider. UI at `https://auth.farh.net`.
  * Authentik credentials are available via the `authentik-credentials` secret in your namespace.
  * Authentik, Auth0, Okta, and Entra-ID should all be supported.
* Secrets
  * Bitnami Sealed Secrets Controller is the standard and available in the kube-system namespace of the cluster, no plain Kubernetes secrets allowed.
  * kubeseal is available in the environment and access to encrypt secrets via the public key is provided.
* Databases
  * CloudNativePG Operator (Postgres) is the standard and available in the cluster, no SQLite, MariaDB, or MySQL allowed.
  * Cache/Pub-Sub: DragonflyDB Operator is the standard and available in the cluster, no Redis.

### Deployment — 2-Stage Flux GitOps

Deployment is fully GitOps-driven. **Do not use `kubectl apply` to deploy application manifests.**

**Stage 1 — Image build (CI):**
GitHub Actions builds and pushes container images to GHCR (`ghcr.io/groombook/api`, `ghcr.io/groombook/web`) on push/PR. Tag format: `YYYY.MM.DD-shortsha`.

**Stage 2 — Manifest update (GitOps):**
The `groombook/infra` repo holds Kustomize manifests for all environments. To deploy, update the image tag(s) in the relevant overlay and commit/merge to `groombook/infra`. Flux (running on the cluster) watches a **cluster repo** (not accessible to agents) that references `groombook/infra` as a **target GitRepository**. Flux reconciles and applies the updated manifests to the cluster automatically.

**Critical rules:**
* `groombook/infra` is a **target GitRepository** — it contains application manifests only. It is **not** a Flux bootstrap or cluster repo. Do not add `flux-system` resources, do not run `flux bootstrap` against it, do not create GitRepository/Kustomization resources within it that point to itself.
* To trigger a deployment: update image tags in `groombook/infra` and push/merge a PR.
* Flux owns convergence — do not `kubectl apply` application manifests directly to drive a release.
* **No Flux Image Automation.** Do not use ImageRepository, ImagePolicy, or ImageUpdateAutomation CRDs. Image tag updates are intentionally driven by CI at push time, not by Flux automation. This is company policy and will not change.

### Dependency & Image Updates — Mend Renovate

**Mend Renovate** is the sole tool for automated dependency and container image updates. Do not configure or use Dependabot — it is not used and will not be used.

* Renovate handles package dependency bumps (npm, Go modules, etc.) and container image tag updates.
* When agents or users ask about automated dependency updates, direct them to Renovate configuration — never suggest Dependabot as an alternative.

### Terraform (OpenTofu) — Flux ToFu Controller

Agents can deploy infrastructure-as-code when a task requires it.

* **How:** Commit OpenTofu (`.tf`) configuration to `groombook/infra` in a dedicated path. The Flux ToFu Controller watches for `Terraform` CRDs and reconciles them automatically — no manual `tofu apply` needed.
* **When to use:** Platform-level provisioning tasks (e.g. Authentik configuration, external DNS records, object storage buckets). Application manifests should remain Kustomize/Helm.
* **Do not** run `tofu` or `terraform` directly against the cluster outside of the controller workflow.
* **Credentials:** Any secrets needed by Tofu workspaces should be provided as Sealed Secrets referenced by the `Terraform` resource.