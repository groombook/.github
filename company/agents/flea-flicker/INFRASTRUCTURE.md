# Infrastructure Information

### Deployment Targets

* Production/Demo
  * Namespace: groombook
  * FQDN: groombook.farh.net
* Development
  * [Namespace: groo](<Namespace: groombook&#xA;FQDN: groombook.farh.net>)mbook-dev
  * FQDN: groombook.dev.farh.net

### Standards

* Kubernetes
  * Cluster Access: Cluster wide read access is granted as is read/write access to -dev namespaces.
  * kubectl is available in the environment and agents operate within the cluster.
* Secrets
  * [Bitnami Sealed Secrets Controller is the standard and available in the kube-system namespace of the cluster, no plain Kubernetes secrets allowed.](<Bitnami Sealed Secrets Controller is the standard and available in the kube-system namespace of the cluster, no plain Kubernetes secrets allowed.>)
  * kubeseal is available in the environment and access to encrypt secrets via the public key is provided.
* Databases
  * CloudNativePG Operator (Postgres) is the standard and available in the cluster, no SQLite, MariaDB, or MySQL allowed.
  * Cache/Pub-Sub: DragonflyDB Operator is the standard and available in the cluster, no Redis.