# DokuWiki

[DokuWiki](https://www.dokuwiki.org) is a standards-compliant, simple to use wiki optimized for creating documentation. It is targeted at developer teams, workgroups, and small companies. All data is stored in plain text files, so no database is required.

## TL;DR;

```console
$ helm install stable/dokuwiki
```

## Introduction

This chart bootstraps a [DokuWiki](https://github.com/bitnami/bitnami-docker-dokuwiki) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

Bitnami charts can be used with [Kubeapps](https://kubeapps.com/) for deployment and management of Helm Charts in clusters. This chart has been tested to work with NGINX Ingress, cert-manager, fluentd and Prometheus on top of the [BKPR](https://kubeprod.io/).

## Prerequisites

- Kubernetes 1.4+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install --name my-release stable/dokuwiki
```

The command deploys DokuWiki on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the DokuWiki chart and their default values.

|              Parameter               |               Description                                  |                   Default                     |
|--------------------------------------|------------------------------------------------------------|-----------------------------------------------|
| `delegateName`               	| Delegate name                                    | `nil`                                         |
| `namespace`            		| Namespace where delegate will be deployed        | `[]` (does not add image pull secrets to deployed pods) |
| `managerHostAndPort`          | Harness service Url                              | `docker.io`                                   |
| `watcherStorageUrl`           | Watcher storage Url                              | `bitnami/dokuwiki`                            |
| `watcherCheckLocation`        | Watcher file name                                | `{TAG_NAME}`                                  |
| `delegateStorageUrl`          | Delegate storage Url                             | `Always`                                      |
| `delegateCheckLocation`       | Delegate file name                               | `[]` (does not add image pull secrets to deployed pods) |
| `accountId`                       | String to partially override dokuwiki.fullname template with a string (will prepend the release name) | `nil` |
| `accountSecret`                   | String to fully override dokuwiki.fullname template with a string                                     | `nil` |
| `delegateProfile`                   | User of the application                                    | `user`                                        |
| `proxyHost`                   | User's full name                                           | `User Name`                                   |
| `proxyPort`                   | Application password                                       | _random 10 character alphanumeric string_     |
| `proxyScheme`                      | User email                                                 | `user@example.com`                            |
| `noProxy`                   | Wiki name                                                  | `My Wiki`                                     |
| `proxyManager`                       | Kubernetes Service type                                    | `LoadBalancer`                                |
| `pollForTasks`                       | Service HTTP port                                          | `80`                                          |
| `helmDesiredVersion`                  | Service HTTPS port                                         | `443`                                         |
| `accountIdShort`             | Kubernetes LoadBalancerIP to request                       | `nil`                                         |
| `proxyUser`      | Enable client source IP preservation                       | `Cluster`                                     |
| `proxyPassword`             | Kubernetes http node port                                  | `""`                                          |

The above parameters map to the env variables defined in [bitnami/dokuwiki](http://github.com/bitnami/bitnami-docker-dokuwiki). For more information please refer to the [bitnami/dokuwiki](http://github.com/bitnami/bitnami-docker-dokuwiki) image documentation.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install --name my-release \
  --set dokuwikiUsername=admin,dokuwikiPassword=password \
    stable/dokuwiki
```

The above command sets the DokuWiki administrator account username and password to `admin` and `password` respectively.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml stable/dokuwiki
```

> **Tip**: You can use the default [values.yaml](values.yaml)

### [Rolling VS Immutable tags](https://docs.bitnami.com/containers/how-to/understand-rolling-tags-containers/)

It is strongly recommended to use immutable tags in a production environment. This ensures your deployment does not change automatically if the same tag is updated with a different image.

Bitnami will release a new chart updating its containers if a new version of the main container, significant changes, or critical vulnerabilities exist.

## Persistence

The [Bitnami DokuWiki](https://github.com/bitnami/bitnami-docker-dokuwiki) image stores the DokuWiki data and configurations at the `/bitnami/dokuwiki` path of the container.

Persistent Volume Claims are used to keep the data across deployments. There is a [known issue](https://github.com/kubernetes/kubernetes/issues/39178) in Kubernetes Clusters with EBS in different availability zones. Ensure your cluster is configured properly to create Volumes in the same availability zone where the nodes are running. Kuberentes 1.12 solved this issue with the [Volume Binding Mode](https://kubernetes.io/docs/concepts/storage/storage-classes/#volume-binding-mode).

See the [Configuration](#configuration) section to configure the PVC or to disable persistence.

## Upgrading

### To 3.0.0

Backwards compatibility is not guaranteed unless you modify the labels used on the chart's deployments.
Use the workaround below to upgrade from versions previous to 3.0.0. The following example assumes that the release name is dokuwiki:

```console
$ kubectl patch deployment dokuwiki-dokuwiki --type=json -p='[{"op": "remove", "path": "/spec/selector/matchLabels/chart"}]'
```
