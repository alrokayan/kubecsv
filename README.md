# `kubecsv`
`kubecsv` is a bash script designed for easy installation of Kubernete clusters and deploying a set of apps and their storage and network from a single comma-separated values (csv) file, `deploy.csv`.

`kubecsv` provides one of the simplest method to create multiple-networks, multiple-home-assistant, multiple-zigbee2mqtt, and multiple-mosquitto in a Kubernetes' cluster to eventually the script deploy a static and dhcp-based pods' network interface.

## Motivation 
The idea behind this project is for homelab newbies to k8s to speed kickstarter their learnings on k8s setup, installation, network (CNI) configuration, and helm app deployments, where the script is divided into five easy-to-follow steps.

Opening a CSV in spreadsheet software (or CSV EDIT extension on VS Code) and then running a single line of code in terminal to have a solid, complete k8s setup with easy reverse engineering the steps is currently a gap.

---
We loved docker-compose.yaml where a simple single file is all you need to deploy your resources. That's in Docker, but what about Kubernetes!? It's a mess! And within a mess, you have versions of different messes! Kompose is limited and requires extra post deployment tasks and adding extra notations to the yaml. After over a year on this topic, I believe we have something similar to that single docker-compose.yaml to Docker; `kubecsv`.

---
## HOW TO CREATE A K8S CLUSTER AND DEPLOY APPS FROM A CSV FILE?

### 1) Download the script
```
git clone https://github.com/alrokayan/kubecsv.git
cd kubecsv
```
or
```
mkdir kubecsv
cd kubecsv
curl -fsSL https://raw.githubusercontent.com/alrokayan/kubecsv/main/kubecsv -o kubecsv
```
### 2) Change script file permission
```
chmod +x kubecsv
```
### 3) Run the script
```
./kubecsv
```
> To create a Kubernetes follow the sequence of steps from `0` to `5` in the *INSTALLATION STEPS* section, run the steps `0` (`apt` uninstall), `1` (`apt` install), `2` (`kubeadm` init/join), `3` (`flannel` and `multus`), `4` (create `deploy.csv`), and `5` (deploy `deploy.csv`). Or `all` to run all steps in sequence *(WARNING: error handling is not implemented)*

---
## EXAMPLES

### 1. Multiple Home Assistant
With this script you can deploy multiple home-assistant and multiple zigbee2mqtt and multiple mqtt brokers, each with a fix IP or dhcp-assigned IP in the same cluster. Storage is nfs for now.

### 2. Homelab
The script takes a reading from [TrueCharts helm charts](https://truecharts.org/charts/description-list/) and deploy them. You can set static IP or dhcp IP with fixed MAC address all within the csv file.

---
## TL;DR
```
curl -fsSL https://raw.githubusercontent.com/alrokayan/kubecsv/main/kubecsv -o kubecsv && chmod +x kubecsv && ./kubecsv all
```

---
---

### MORE DETAILS ABOUT THE PROJECT [HERE](https://github.com/alrokayan/kubecsv/blob/main/READMORE.md) ...