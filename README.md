# PostgreSQL Operator
v1.4, {docdate}

## Table of Contents

* <<Overview>>
* <<Requirements>>
* <<Build and Setup Instructions>>
* <<Configuration>>
* <<Examples>>
* <<PostgreSQL Operator Container>>

[#Overview]
## Overview

The PostgreSQL Operator provides a Kubernetes operator capability for managing PostgreSQL Clusters deployed within a Kubernetes.


The PostgreSQL Operator leverages Kubernetes Third Party Resources to define custom resource type **pgcluster**, **pgbackups**, and **pgupgrades**.

Once those custom objects are defined, Kubernetes provides the ability to create and manage those objects similar to any other native Kubernetes object.

The PostgreSQL Operator runs within Kubernetes detecting these new custom object types being created or removed.

Once the objects are detected, the PostgreSQL Operator enables users to perform operations across the Kubernetes environment, including:

* Create Cluster
* Destroy Cluster
* Backup Cluster
* Scale a Cluster
* Restore Cluster
* Upgrade Cluster
* View PVC
* Test Connections
* Clone a Cluster
* Create a Policy
* Apply a Policy

What actually gets created on the Kube cluster for a
**pgcluster** resource is defined as a **deployment strategy**.  Strategies
are documented in detail in [Deployment Strategies](./docs/strategies.asciidoc)

[#Requirements]
## Requirements

* Kubernetes 1.5.3+
* [PostgreSQL 9.5+ Container](https://hub.docker.com/r/crunchydata/crunchy-postgres/)
* [PostgreSQL Backup Container](https://hub.docker.com/r/crunchydata/crunchy-backup/)
* [PostgreSQL Upgrade Container](https://hub.docker.com/r/crunchydata/crunchy-upgrade/)
* For OpenShift deployments, Openshift Origin 1.5.1+ or Openshift Container Platform 3.5

[#Build and Setup Instructions]
## Build and Setup Instructions

With the operator deployed, the **pgo** command line
interface can execute commands that the **postgres-operator** understands
and reacts to.

You can download a pre-built **pgo** CLI binary from
the [Releases Page](https://github.com/CrunchyData/postgres-operator/releases)
it yourself using the build instructions, documented on the [Build and Setup page](./docs/build.asciidoc)

[#Configuration]
## Configuration

You can configure both the client and the operator.  The
configuration options are documented on the [Configuration page](./docs/config.asciidoc)

[#Examples]
## Examples

Some examples of using the **pgo** command line interface are as follows.

.Display Cluster Information
```bash
pgo show cluster all
pgo show cluster db1 db2 db3
pgo show cluster mycluster
pgo show cluster mycluster --show-secrets=true
```

.Create Cluster
```bash
pgo create cluster mycluster
```

.Scale Cluster
```bash
pgo scale mycluster --replica-count=2
```

.Delete a Cluster
```bash
pgo delete cluster mycluster
```

.Backup Cluster
```bash
pgo create backup mycluster
```

.Restore Cluster
```bash
pgo create cluster myrestore --secret-from=foo --backup-pvc=mypvc --backup-path=foo-backups/2017-03-21-15-57-21
```

.Upgrade Cluster (minor Postgres version upgrade)
```bash
pgo create upgrade mycluster
```

.Upgrade Cluster (major Postgres version upgrade from 9.5 to 9.6)
```bash
pgo create upgrade mycluster --upgrade-type=major
```

.View PVC
```bash
pgo show pvc mypvc
```

.Test Connections
```bash
pgo test mycluster
```

.Clone Cluster
```bash
pgo clone mycluster --name=myclone
```

.Create a Policy
```bash
pgo create policy policy1 --in-file=./policy1.sql
pgo create policy policy1 --url=https://someurl/policy1.sql
```

.Apply a Policy
WARNING:  policies are POWERFUL and executed as the superuser in PostgreSQL
which allows for any sort of SQL to be executed.
```bash
pgo apply policy1 --selector=name=mycluster
```

Details on the **pgo** commands are found in the 
[User Guide](./docs/user-guide.asciidoc)

[#PostgreSQL Operator Container]
## PostgreSQL Operator Container

In the following diagram, the postgres operator client, **pgo**, is
shown interacting with the postgres operator that runs within
a Kubernetes cluster.  The operator is responsible for creating
or modifying PostgreSQL databases deployed within the Kube cluster.

[diagram]: ./docs/operator-diagram.png

The operator funtionality runs in a Pod deployed to your
Kubernetes cluster.  The **postgres-operator** Docker container
is available on [Dockerhub](https://hub.docker.com/r/crunchydata/postgres-operator/)

You can also build the Docker image for **postgres-operator** using
the build instructions located on the [Build and Setup](./docs/build.asciidoc)
