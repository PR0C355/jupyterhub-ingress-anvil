# JupyterHub with Ingress - Helm Chart

This Helm chart deploys a JupyterHub instance with an Ingress resource. JupyterHub is a multi-user Jupyter Notebook server that allows multiple users to access and run Jupyter Notebooks on a shared cluster. This chart was designed for the [Anvil Cluster](https://doi.org/10.1145/3491418.3530766) but should be applicable to any Kubernetes deployment.

## Prerequisites

Before deploying this Helm chart, ensure that you have the following prerequisites:

- Kubernetes cluster with Helm installed
- Persistent storage provisioner for user notebooks (e.g., a dynamic provisioner like `ReadWriteMany`)

## Installation

1. Clone the repository:

   ```shell
   git clone https://github.com/PR0C355/jupyterhub-ingress-anvil.git
   ```

2. Customize the values in `values.yaml` file based on your requirements. You can modify anything normally modifiable in [JupyterHub](https://z2jh.jupyter.org/en/stable/resources/reference.html#helm-chart-configuration-reference), as well as the deployed ingress resource. 

3. Install the Helm chart:

   ```shell
   helm upgrade --cleanup-on-fail \
     --install jupyterhub ./jupyterhub-ingress-anvil \
     --namespace oo-test \
     --create-namespace
   ```

## Configuration

The following table lists the configurable parameters of the ingress resource in the JupyterHub Helm chart and their default values:

| Parameter                                    | Description                             | Default                  |
| -------------------------------------------- | --------------------------------------- | ------------------------ |
| `ingress.enabled`                            | Enable Ingress resource creation         | `true`                   |
| `ingress.annotations`                        | Ingress annotations                      | `{}` (unspecified)       |
| `ingress.name`                               | Name of the Ingress resource             | `jhub-ingress`           |
| `ingress.labels`                             | Labels for the Ingress resource          | `{}` (unspecified)       |
| `ingress.rules`                              | Rules for defining host, paths, and backend | `[]` (unspecified)    |
| `ingress.rules[].host`                       | Hostname for the Ingress rule            | (none)                   |
| `ingress.rules[].httpPaths`                  | HTTP paths and backend services for the rule | `[]` (unspecified)  |
| `ingress.rules[].httpPaths[].path`            | Path for the Ingress rule                | (none)                   |
| `ingress.rules[].httpPaths[].pathType`        | Path type for the Ingress rule           | (none)                   |
| `ingress.rules[].httpPaths[].backend.serviceName` | Name of the backend service          | (none)                   |
| `ingress.rules[].httpPaths[].backend.servicePortNumber` | Port number of the backend service | (none)          |

Please note that the `(none)` values indicate parameters that are dependent on user configuration and are not provided in the given Ingress file.

## Usage

Once the Helm chart is deployed, you can access JupyterHub by navigating to the specified Ingress hostname in your web browser. You can authenticate using the configured authentication method (e.g., Dummy, GitHub, CILogon, etc.) and start using Jupyter Notebooks.

## Upgrading

To upgrade the JupyterHub deployment with a new version of the Helm chart, use the following command:

```shell
helm upgrade --cleanup-on-fail \
   jupyterhub ./jupyterhub-ingress-anvil
```

Ensure to modify the `values.yaml` file if you want to make any configuration changes during the upgrade.

## Uninstallation

To uninstall the JupyterHub deployment, use the following command:

```shell
helm uninstall jupyterhub
```

This will remove all resources associated with the JupyterHub deployment, including the Ingress resource and user notebooks PVCs.

## Limitations

- This Helm chart does not handle automatic scaling of the JupyterHub deployment. You may need to manually adjust the number of replicas based on the workload.
- It is recommended to secure the JupyterHub deployment with appropriate authentication and authorization mechanisms to control user access and prevent unauthorized usage.
- Ensure that your Kubernetes cluster has sufficient resources (CPU, memory, storage) to accommodate the desired number of JupyterHub replicas and user notebooks.
