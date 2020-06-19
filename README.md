# K3s

This Ansible role will download and install the lightweight Kubernetes
distribution K3s. The role installs K3s [using the k3s binary](https://rancher.com/docs/k3s/latest/en/installation/install-options/#installing-k3s-from-the-binary)
itself. Additionally, the role supports some more advanced configuration of K3s
using variables described below.

One goal of this role is to support the [high availability](https://rancher.com/docs/k3s/latest/en/installation/ha-embedded/)
mode of K3s with a multi-node control plane. The HA mode is not enabled by
default. Worker-only nodes are not yet supported.

## Requirements

This role has only been tested with Debian, but should support Ubuntu, RHEL,
and CentOS.

* Ansible - 2.9+ recommended
* kubectl - required for setting kubeconfig (default)

## Variables

The following variables are supported. When using this role, set variables
inside the role context. For example:

```yaml
  roles:
  - name: k3s
    vars:
      k3s_cluster_name: mycluster
```

### `k3s_cluster_name`

- Set the name of the cluster
- Default: *k3s*

### `k3s_version`

- Specify the K3s release version to install
- Default: *v1.17.3+k3s1*

### `k3s_set_kubeconfig`

- When set, the local kubeconfig on the control host will have the K3s cluster
added as a new context (requires kubectl)
- Default: *true*

### `k3s_cidr_cluster`

- Network IPs to use for the pod network
- Default: *10.42.0.0/16*

### `k3s_cidr_service`

- Network IPs to use for the service network
- Default: *10.43.0.0/16*

### `k3s_systemd_directory`

- Base folder for systemd configuration
- Default: */etc/systemd/system*

### `k3s_enable_traefik`

- Deploy Traefik as an ingress controller
- Default: *true*

### `k3s_enable_servicelb`

- Deploy the basic [service load balancer](https://rancher.com/docs/k3s/latest/en/networking/#service-load-balancer)
- Default: *true*

### `k3s_ha`

- Deploy K3s in high availability cluster mode using the embedded Dqlite database
- Default: *false*

### `k3s_custom_cni`

- Configure K3s to support installation of custom CNI plugins
  - By default, Flannel will be configured to get the cluster up and running
- Default: *false*

### `k3s_cni_version`

- Specify the [CNI plugins](https://github.com/containernetworking/plugins/releases) version to install
  - Only used when `k3s_custom_cni` is *true*
- Default: *v0.8.6*

### `k3s_datastore_endpoint`

- Specify the connection string for an external datastore
  - This replaces the built-in SQLite datastore
  - See the [datastore endpoint](https://rancher.com/docs/k3s/latest/en/installation/datastore/#datastore-endpoint-format-and-functionality) for formatting requirements
- Default: ''

### `k3s_datastore_cafile`

- The location of the file containing the public certificate of the datastore's Certificate Authority (CA) for datastores using TLS
  - This role does **not** create this file
- Default: ''

### `k3s_datastore_certfile`

- The location of the file containing the public certificate used by the client when connecting to the datastore
  - This role does **not** create this file
- Default: ''

### `k3s_datastore_keyfile`

- The location of the file containing the private key used by the client when connecting to the datastore
  - This role does **not** create this file
- Default: ''
