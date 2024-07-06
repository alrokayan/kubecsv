# kubecsv

This script provides a command-line interface for installing and managing a Kubernetes cluster on set of Ubuntu 22.04 hosts using the kubeadm initialization method. The script is divided into several steps, and users can choose to run individual steps or the entire installation process at once. The script do:
- Uninstall Everything: Removes all installed packages and files.
- Prepare hosts and install pre-requisites (apt): Configures the hosts, installs required packages, and creates necessary directories.
- Install kubernetes (kubeadm): Installs the kubernetes tools on the master node and initializes the cluster using kubeadm.
- Deploy kubernetes network (flannel and multus): Deploys the flannel CNI plugin and configures multus for network plugins.
- Generate deploy.csv: Creates a deploy.csv file with the configurations for deploying the applications.
- Deploy deploy.csv: Deploys the applications based on the deploy.csv file.

Additionally, the script provides options for installing binaries locally, freeing up space, copying the kubeconfig file from the master node, fixing the DNS, and running network diagnostic tools.

## How To
On an empty folder on mac or linux, run:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/alrokayan/kubecsv/main/kubecsv"
```

# Overview of `kubecsv`Script

The `kubecsv` script appears to be a utility designed for managing Kubernetes clusters, specifically tailored for initialization, configuration, and cleanup tasks. It provides a user-friendly interface for executing a series of operations related to Kubernetes cluster management, including installation of prerequisites, cluster creation, network deployment, and application deployment. The script outputs its logs to a designated file and uses file descriptor 3 (`>&3`) for logging, indicating a sophisticated handling of output streams.

## Detailed Steps Description

### Step 0: Uninstall Everything
- **Description**: This step likely involves cleaning up or uninstalling all components related to the Kubernetes cluster that were previously installed or configured by this script. It might include removing installed packages, deleting configuration files, and cleaning up any temporary files created during the process.

### Step 1: Prepare Hosts and Install Pre-requisites
- **Action**: Executes necessary commands to prepare the host machines for Kubernetes installation. This includes updating package lists, installing required packages via `apt` (indicating a Debian-based system), and possibly setting up necessary system configurations.
- **Tools Used**: Likely uses `apt` for package management.

### Step 2: Create Kubernetes Cluster
- **Action**: Initializes a Kubernetes cluster using `kubeadm init`. This step sets up the control plane and prepares the cluster for adding worker nodes.
- **Tools Used**: Utilizes `kubeadm`, a tool provided by Kubernetes for creating and managing clusters.

### Step 3: Deploy Kubernetes Network
- **Action**: Deploys networking solutions within the Kubernetes cluster, specifically mentioning `flannel` and `multus`. Flannel is a simple and easy-to-configure layer 3 network fabric designed for Kubernetes, while Multus is a CNI plugin that enables attaching multiple network interfaces to pods.
- **Tools Used**: Kubernetes command-line tools, possibly `kubectl` for applying network configurations.

### Step 4: Generate `deploy.csv` (Example Content)
- **Action**: Generates a CSV file named `deploy.csv`, which likely contains configuration or deployment specifications for applications or services to be deployed within the cluster. The mention of "example content" suggests this step might produce a template or sample file for users to customize.
- **Tools Used**: Scripting operations to create and populate the CSV file.

### Step 5: Deploy `deploy.csv`
- **Action**: Takes the previously generated `deploy.csv` file and deploys its contents to the Kubernetes cluster. This step might involve parsing the CSV file and applying the configurations it contains, such as deploying applications or services specified within.
- **Tools Used**: Likely involves `kubectl` for deploying resources defined in the CSV to the Kubernetes cluster.

## Additional Commands

- **`(un3)`**: Undoes the actions performed in steps 3 and 5, effectively removing all applications and the Kubernetes network configuration.
- **`(un5)`**: Specifically targets the undoing of step 5, removing all applications deployed from the `deploy.csv` without affecting the network configuration.

The script provides a comprehensive toolset for managing the lifecycle of a Kubernetes cluster, from preparation and creation to deployment and cleanup, with a focus on ease of use and automation.

## pre-requisitess
Local machine (the machine that will run the script) requirements:
- Mac or Linux

The script will download and run binaries (helm, kubectl, jq and k9s) in the bin folder where you are running the script. 

Hosts (aka Nodes) requirements:
- Ubuntu 22.04 VM or bare-metal

The script works for one node or more.

## use-cases

### Multiple Home Assistant with discovery in one k8s cluster
With this script you can deploy multiple home-assistant and multiple zigbee2mqtt and multiple mqtt brokers, each with a fix IP or dhcp-assigned IP

## Links:
- Charts List: https://truecharts.org/charts/description-list/
- Charts values: https://github.com/truecharts/charts/tree/master/charts
- Common values: https://github.com/truecharts/library-charts/blob/main/library/common/values.yaml

