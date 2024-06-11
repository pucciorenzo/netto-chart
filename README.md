# Netto deployment in K8s
This tutorial allow to install [Netto] (https://github.com/miolad/netto) in a kubernetes cluster with minimal permissions using Helm.
## Requirements
- Linux kernel version: **5.5**
- Netto container run in `--privileged` mode
## Install Netto
To deploy Netto in your cluster run:
```
git clone https://github.com/pucciorenzo/netto-chart.git
cd netto-chart
```
In `values.yml` change `address` with the given value.

Create a namespace for Netto in the cluster
```
kubectl create namespace netto
```
And install Netto
```
helm install netto . -n netto 
```
