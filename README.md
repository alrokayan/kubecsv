# kubecsv

The `kubecsv` script is a utility script runs on MacOS and Linux designed for easy installation of Kubernetes cluster and deploying apps from a simple csv file, deploy.csv (check examples) on set of Ubuntu 22.04 host(s). It has beeen tested on xcp-ng VMs and bare-metal.

## Helpers
This shell script is using:
1- **kubeadm**: for kubernetes installation
2- **Flannel** and **Multus**: for CNI (Container Network Interface) deployment
3- **kubectl** and **helm**: for managment and deployment
4- **Truecharts**: for helm charts apps
5- **k9s**: for monitoring
> The script will download and run binaries (helm, kubectl, jq and k9s) in the bin folder where you are running the script. 

## TL;DR
```
curl -fsSL https://raw.githubusercontent.com/alrokayan/kubecsv/main/kubecsv -o kubecsv && chmod +x kubecsv && ./kubecsv
```
Then go throug the steps 0 to 5, using an interactive interface.

## CSV Columns
The file must be named `deploy.csv` and put in the same directory where you are ruuning `kubecsv`. The CSV columns description are:

1. `app_name`: This column hold a the app name, you can put any
2. `helm_truechart`: This column holds the app helm chart name from this [link](https://truecharts.org/charts/description-list/) or list below.
3. `storage_name`: This column contains the name of the storage. It can be any, however, some times you want to overwrite a named storage. You can see all named storages from the values.yaml in [TrueCharts github repo](https://github.com/truecharts/charts/tree/master/charts)
4. `storage_enabled`: This column is to disable a named storage (OPTIONAL)
5. `storage_path`: This column contains the NFS path of the app 
6. `storage_subPath`: This column contains the sub path follows the `storage_enabled`. As a best practise, it's better to name the `storage_subPath` as `storage_name` (OPTIONAL)
7. `storage_server`: This column for NFS server
8.  `storage_type`: This column to indicate the storage type, for now it has been tested on `nfs` only 
9.  `storage_mountPath`: This column contains the path of data/config inside the container
10. `nw_master_nic`: This column for MacVLAN (dhcp or static ip) master/parent network interface
11. `nw_mac`: This column to fix the MAC addreess of the app, useful for dhcp to assign fixed ip from the dhcp server
12. `nw_address_with_subnet`: This column is optional. If you assign an IP/CIDR value `kubecsv` will attach a static IP, if left empty it will use dhcp (OPTIONAL) 
13. `nw_gateway`: This column is needed only if you assign fixed IP in `nw_address_with_subnet`. It holds the gateway for the provided fix ip. (OPTIONAL)
14. `nw_dns1`: This column is needed only if you assign fixed IP in `nw_address_with_subnet`. It holds DNS server one (OPTIONAL) 
15. `nw_dns2`: This column is needed only if you assign fixed IP in `nw_address_with_subnet`. It holds DNS server two (OPTIONAL) 
16. `run_as_user`: This column is optional too if you want to set a UID for the app to run as (OPTIONAL)
17. `run_as_group`: This column is optional too if you want to set a GID for the app to run as (OPTIONAL)
18. `privileged`: This column contains true or false. true means the app will run in a `privileged` mode. Default is false. (OPTIONAL)
19. `extra_helm_values`: This column contains any extra values you want to add in a form of `--set key:value` (OPTIONAL)

## TrueChart Charts
`kubecsv` supports almost all of the 700+ TrueCharts charts. You can view them here: https://truecharts.org/charts/description-list/

## Options:
- `./kubecsv` will an interactive script with a set of tools, where you can choose to deploy and step and or run any of the tools
- `./kubecsv 0` will run step 0 (uninstalling), see details below (non-interactive)
- `./kubecsv 1` will run step 1 (installing), see details below (non-interactive)
- `./kubecsv 2` will run step 2 (create the cluster), see details below(non-interactive)
- `./kubecsv 3` will run step 3 (deploy k8s network), see details below(non-interactive)
- `./kubecsv 4` will run step 4 (generate an example deploy.csv), see details below(non-interactive)
- `./kubecsv 5` will run step 5 (deploy deploy.csv), see details below(non-interactive)
- `./kubecsv all` will run the steps 0 to 5, one by one(non-interactive)
- `./kubecsv un3` Will undoes the actions performed in steps 3 and 5, effectively removing all applications and the Kubernetes network configuration (non-interactive)
- `./kubecsv un5` Will specifically targets the undoing of step 5, removing all applications deployed from the `deploy.csv` without affecting the network configuration (non-interactive)
- `./kubecsv k` Will run k9s monitoring tool using the configured kubeconfig 

## Logs
Every time you run the script, a `logs` folder will be created contains your current and past logs

## The Five Steps 
### Step 0: Uninstall Everything
- This step involves cleaning up or uninstalling all components related to the Kubernetes cluster that were previously installed or configured by this script. It might include removing installed packages, deleting configuration files, and cleaning up any temporary files created during the process.

### Step 1: Prepare Hosts and Install Pre-requisites
- Executes necessary commands to prepare the host machines for Kubernetes installation. This includes updating package lists, installing required packages via `apt`, and possibly setting up necessary system configurations.

### Step 2: Create Kubernetes Cluster
- Initializes a Kubernetes cluster using `kubeadm init`. This step sets up the control plane and prepares the cluster for adding worker nodes. and execute `kubeadm join` command on the workers nodes.

### Step 3: Deploy Kubernetes Network
- Deploys networking solutions within the Kubernetes cluster, specifically mentioning `flannel` and `multus`. Flannel is a simple and easy-to-configure layer 3 network fabric designed for Kubernetes, while Multus is a CNI plugin that enables attaching multiple network interfaces to pods.

### Step 4: Generate `deploy.csv` (Example Content)
- Generates a CSV file named `deploy.csv`, which contains configuration or deployment specifications for applications or services to be deployed within the cluster. You can see it as a template or sample file for users to customize.

### Step 5: Deploy `deploy.csv`
- Takes the previously generated (or provided) `deploy.csv` file and deploys its contents to the Kubernetes cluster. This step involve parsing the CSV file and applying the configurations it contains, such as deploying applications, storage and network.

## Requirements
Mac or Linux as a local machine (the machine that will run the script). Regarding the hosts (nodes) the script has been tested on Ubuntu 22.04 VM and bare-metal. The script works for one node or multiple.

## Example
### 1. Multiple Home Assistant
With this script you can deploy multiple home-assistant and multiple zigbee2mqtt and multiple mqtt brokers, each with a fix IP or dhcp-assigned IP in the same cluster. Storage is nfs for now.

![multi-home-assistant.csv.png](/assets/images/csv/multi-home-assistant.csv.png)
*A deployment csv file to deploy multiple home-assistant, multiple zigbee2mqtt, and multiple mqtt brokers with dhcp and static ip*

![multi-home-assistant.csv.png](/assets/images/csv-edit/multi-home-assistant.csv.png)
*That's how the csv file looks like openning it with janisdd.vscode-edit-csv*

![multi-home-assistant.csv.png](/assets/images/result/multi-home-assistant.csv.png)
*That's the result after deployment of multi-home-assistant csv file*

### 2. Homelab
The script takes a reading from [truecharts helm charts](https://truecharts.org/charts/description-list/) and deploy them. You can set static IP or dhcp IP with fixed MAC address all within the csv file.

![homelab.csv.png](/assets/images/csv/homelab.csv.png)
*A deployment csv file to deploy a set of homelab apps with dhcp and static ip*

![homelab.csv.png](/assets/images/csv-edit/homelab.csv.png)
*That's how the csv file looks like openning it with janisdd.vscode-edit-csv*

![homelab.csv.png](/assets/images/result/homelab.csv.png)
*That's the result after deployment of homelab csv file*

## Useful links:
1. Charts List: https://truecharts.org/charts/description-list/
2. Charts values: https://github.com/truecharts/charts/tree/master/charts
3. Common values: https://github.com/truecharts/library-charts/blob/main/library/common/values.yaml
4. Recommended stuido code extension to edit CSV (janisdd.vscode-edit-csv): https://marketplace.visualstudio.com/items?itemName=janisdd.vscode-edit-csv

---
> `Apache kubecsv`
> `Copyright 2024 The Apache Software Foundation`
> ` `
> `This product includes software developed at`
> `The Apache Software Foundation (http://www.apache.org/).`