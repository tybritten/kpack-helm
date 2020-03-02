# kpack Helm Chart
A helm chart to install [`kpack`](https://github.com/pivotal/kpack).


## Prerequisites Details

1. A Kubernetes cluster version 1.14 or later
1. [kubectl CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
1. Cluster-admin permissions for the current user
1. Accessible Docker V2 Registry


## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ git clone https://github.com/tybritten/kpack-helm.git
$ helm install my-release kpack-helm/kpack
```

Ensure that the kpack controller & webhook have a status of `Running` using  `kubectl get`.   

```bash
$ kubectl get pods --namespace kpack --watch
```

## Deleting the Charts

Delete the Helm deployment as normal

```
$ helm delete my-release
```

*Note:* This Chart includes CRDs which Helm3 will not delete automatically. To remove these manually, type:

```bash
$ kubectl delete -f kpack/crds/release-0.0.6.yaml
```


## Configuration

The following table lists the configurable parameters of the `kpack` chart and their default values. Update the `values.yaml` with the desired options. You do not need `git` credentials for public code repositories.

|              Parameter               |                             Description                             |                       Default                       |
| ------------------------------------ | ------------------------------------------------------------------- | --------------------------------------------------- |
| `nameOverride`                       | Override the name of the Chart                                      | `""`                                                |
| `fullnameOverride`                   | Override the full name of the Chart                                 | `""`                                                |
| `clusterbuilder.name`                | Name for the clusterbuilder resource                                | `default`                                           |
| `clusterbuilder.image`               | default CNB builder                                                 | `cloudfoundry/cnb:bionic`                           |
| `secrets.registries.name`            | Name of the container registry you want to configure                | `""`                                                |
| `secrets.registries.address`         | Address of the registry same as the begining of the tag for non-docker.io registries                                 | `""`                                                |
| `secrets.registries.type`            | Type of container registry- currently supports Google CR and docker-compatible                | `""`                                                |
| `secrets.registries.username`        | Username for the registry login, only needed for docker-compatible  | `""`                                                |
| `secrets.registries.password`        | Password for the registry login, only needed for docker-compatible  | `""`                                                |
| `secrets.registries.json_file`       | Path to the google JSON authenticaion file, needed for GCR. put in the secrets folder          | `secrets/secret.json`                                                |
| `secrets.git.name`                   | Name of the git login secrets you want to configure                   | `"github"`                                                |
| `secrets.git.type`            | Type of git credentials- basic, ssh, token                | `""`                                                |
| `secrets.git.username`        | Username for the registry, required for basic. For token authentication put your token here  | `""`                                                |
| `secrets.git.password`        | Password for the registry login, only needed for basic  | `""`                                                |
| `secrets.git.ssh_privatekey`       | Path to the ssh private key file for ssh login. put in the secrets folder          | `secrets/id_rsa`                                                |

With these settings, `kpack` is setup and you can create image builds with `Image` manifests. If you want to also configure those builds as part of the helm deployment, also configure it in the `values.yaml.` It currently supports github repositories and blob as [source options](https://github.com/pivotal/kpack/blob/master/docs/image.md).

|              Parameter               |                             Description                             |                       Default                       |
| ------------------------------------ | ------------------------------------------------------------------- | --------------------------------------------------- |
| `images.git.name`        | Name for the git source Image  | `""`                                                |
| `images.git.source`        | The git repository url. For now, only https repositories are supported.   | `""`                                                |
| `images.git.revision`        | The git revision to use. This value may be a commit sha, branch name, or tag.   | `""`                                                |
| `images.git.SubPath`        | A subdirectory within the source folder where application code resides. Can be ignored if the source code resides at the `root` level.   | `""`                                                |
| `images.blob.name`        | Name for the blob source Image  | `""`                                                |
| `images.git.source`        | The URL of the source code blob. This blob needs to either be publicly accessible or have the access token in the URL   | `""`                                                |
| `images.git.SubPath`        | A subdirectory within the source folder where application code resides. Can be ignored if the source code resides at the `root` level.   | `""`                                                |


