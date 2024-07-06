# kubecsv
This script provides a command-line interface for installing and managing a Kubernetes cluster on set of Ubuntu 22.04 hosts using the kubeadm initialization method. The script is divided into several steps, and users can choose to run individual steps or the entire installation process at once.

Here's a brief explanation of the script's steps:
- Uninstall Everything: Removes all installed packages and files.
- Prepare hosts and install pre-requisites (apt): Configures the hosts, installs required packages, and creates necessary directories.
- Install kubernetes (kubeadm): Installs the kubernetes tools on the master node and initializes the cluster using kubeadm.
- Deploy kubernetes network (flannel and multus): Deploys the flannel CNI plugin and configures multus for network plugins.
- Generate deploy.csv: Creates a deploy.csv file with the configurations for deploying the applications.
- Deploy deploy.csv: Deploys the applications based on the deploy.csv file.

Additionally, the script provides options for installing binaries locally, freeing up space, copying the kubeconfig file from the master node, fixing the DNS, and running network diagnostic tools.

## use-cases

### Multiple Home Assistant with discovery in one k8s cluster

## Ho To
On  mac or linux, run:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/alrokayan/kubecsv/main/kubecsv"
```

## Links:
- Charts List: https://truecharts.org/charts/description-list/
- Charts values: https://github.com/truecharts/charts/tree/master/charts
- Common values: https://github.com/truecharts/library-charts/blob/main/library/common/values.yaml

