# Graph Protocol Indexer
Helm chart deploying Graph Protocol Indexer.

## TL;DR

```console
$ helm repo add vulcanlink https://vulcanlink.github.io/charts/
$ helm install my-release vulcanlink/graphprotocol-indexer
```

## Introduction

This chart bootstraps a graphprotocol-indexer deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

Vulcan Link charts can be used with [Kubeapps](https://kubeapps.com/) for deployment and management of Helm Charts in clusters. This chart has been tested to work with NGINX Ingress on top of AWS EKS.

## Prerequisites

- Kubernetes 1.12+
- Helm 2.12+ or Helm 3.0-beta3+
- PV provisioner support in the underlying infrastructure

## Initial setup

Identify the Kubernetes worker nodes that should be used to execute
user containers.  Do this by labeling each node with
`graphprotocol/node-role=index-role` and `graphprotocol/node-id=index-node-<ID>` 
For a single node cluster, simply do
```shell
kubectl label nodes --all graphprotocol/node-role=index-role graphprotocol/node-id=index-node-0
```

## Installing the Chart
To install the chart with the release name `my-release`:

```console
$ helm install my-release vulcanlink/graphprotocol-indexer
```

The command deploys graphprotocol-indexer on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components but PVC's associated with the chart and deletes the release.

To delete the PVC's associated with `my-release`:

```console
$ kubectl delete pvc -l release=my-release
```

> **Note**: Deleting the PVC's will delete blockchain data as well. Please be cautious before doing it.

## Parameters

The following tables lists the configurable parameters of the graphprotocol-indexer chart and their default values.

|                   Parameter                   |                                                                                Description                                                                                |                            Default                            |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| `global.imageRegistry`                        | Global Docker Image registry                                                                                                                                              | `nil`                                                         |
| `global.imagePullSecrets`                     | Global Docker registry secret names as an array                                                                                                                           | `[]` (does not add image pull secrets to deployed pods)       |
| `global.storageClass`                         | Global storage class for dynamic provisioning                                                                                                                             | `nil`                                                         |
| `image.registry`                              | Image registry                                                                                                                                                            | `docker.io`                                                   |
| `image.repository`                            | Image name                                                                                                                                                                | `graphprotocol/graph-node`                                          |
| `image.tag`                                   | Image tag                                                                                                                                                                 | `{TAG_NAME}`                                                  |
| `image.pullPolicy`                            | Image pull policy                                                                                                                                                         | `IfNotPresent`                                                |
| `image.pullSecrets`                           | Specify Image pull secrets                                                                                                                                                | `nil` (does not add image pull secrets to deployed pods)      |
| `image.command`                               | Specify Image run command                                                                                                                                       | `nil`                                                    |
| `image.args`                                  | Specify Image run command args                                                                                                                                  | `nil` |                                                      |
| `nameOverride`                                | String to partially override graphprotocol-indexer.fullname template with a string (will prepend the release name)                                                                   | `nil`                                                         |
| `fullnameOverride`                            | String to fully override graphprotocol-indexer.fullname template with a string                                                                                                       | `nil`                                                         |
| `volumes.data.mountPath`                      | Blockchain data mountpath                                                                                                                                                 | `/var/lib/graph`                                             |
| `container.http`                              | graphprotocol-indexer Container http port                                                                                                                                         | `8000`                                                        |
| `container.jsonRpc`                                | graphprotocol-indexer Container JSON-RPC port                                                                                                                                    | `8020`                                                        |
| `container.metrics`                                | graphprotocol-indexer Container Prometheus metrics port                                                                                                                                    | `8040`                                                        |
| `service.type`                                | Kubernetes Service type                                                                                                                                                   | `ClusterIP`                                                   |
| `service.http`                                | graphprotocol-indexer Service http port                                                                                                                                           | `8000`                                                        |
| `service.ws`                                  | graphprotocol-indexer Service JSON-RPC port                                                                                                                                      | `8020`                                                        |
| `service.metrics`                                  | graphprotocol-indexer Service Prometheus metrics port                                                                                                                                      | `8040`                                                        |
| `service.nodePort`                            | Kubernetes Service nodePort                                                                                                                                               | `nil`                                                         |
| `service.annotations`                         | Annotations for graphprotocol-indexer service                                                                                                                                              | `{}` (evaluated as a template)                                |
| `persistence.enabled`                         | Enable persistence using PVC                                                                                                                                              | `true`                                                        |
| `persistence.existingClaim`                   | Provide an existing `PersistentVolumeClaim`, the value is evaluated as a template.                                                                                        | `nil`                                                         |
| `persistence.storageClass`                    | PVC Storage Class for graphprotocol-indexer volume                                                                                                                                   | `nil`                                                         |
| `persistence.accessModes`                     | PVC Access Mode for graphprotocol-indexer volume                                                                                                                                     | `[ReadWriteOnce]`                                             |
| `persistence.size`                            | PVC Storage Request for graphprotocol-indexer volume                                                                                                                                 | `300Gi`                                                       |
| `persistence.annotations`                     | Annotations for the PVC                                                                                                                                                   | `{}`                                                          |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install my-release \
  --set persistence.enabled=false \
    vulcanlink/graphprotocol-indexer
```

The above command disables persistent storage, meaning the node will have to resync after every Pod restart/deletion.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```console
$ helm install my-release -f values.yaml vulcanlink/graphprotocol-indexer
```

> **Tip**: You can use the default [values.yaml](values.yaml)