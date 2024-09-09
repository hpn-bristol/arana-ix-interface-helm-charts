# Ix Interface

## Installation

Ix can be easily installed on a Kubernetes cluster with the following steps. Make sure to follow these instructions carefully in order to avoid any issues.

1. Go to the `/charts` subfolder.
    ```bash
    cd charts
    ```
2. Install Ix Interface using helm.
    ```bash
    helm install -n SET-YOUR-NAMESPACE --set global.namespace=SET-YOUR-NAMESPACE --set ixhost=http://$(kubectl get nodes -o jsonpath='{.items[0].status.addresses[0].address}'):31234 ix-interface ix-interface
    ```
    ***NOTICE:** Make sure to set your desired namespace for both the -n flag and the `global.namespace` value.*

### Services

The following services will be created during the installation process. They enable communication between Ix and its components and also expose Ix outside of the cluster.

| hostname     | type      | port  | description                                                                                                                           |
| ------------ | --------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------- |
| ix-interface | NodePort  | 31234 | Reserves node's **31234** port and routes its traffic to Ix. This means the exposed endpoint for Ix will be `http://<node-ip>:31234`. |
| ix-mongodb   | ClusterIP | 27017 | Service for internal communication between Ix and MongoDB pods.                                                                       |
| ix-mysql     | ClusterIP | 3306  | Service for internal communication between Ix and MySQL pods.                                                                         |

### Other dependencies

A few other dependencies will also be automatically deployed during the installation process. If you browse the helm chart templates you can see that these  dependencies include: Persistent Volumes, Persistent Volume Claims, Config Maps and Secret files.

## Updating

When making changes to the deployment, you can use this command to update the pods while preserving the values that were given during the installation process.

```bash
helm upgrade ix-interface ix-interface --reuse-values -n SET-YOUR-NAMESPACE
```

## Removing Ix

If you want to remove the deployed Ix components, you can easily do that with the use of `helm delete` command.

```bash
helm delete ix-interface -n SET-YOUR-NAMESPACE
```
