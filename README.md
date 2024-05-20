# Netto deployment in K8s
This tutorial allow to install [ Netto ](https://github.com/miolad/netto/tree/sample-only) in a kubernetes cluster with minimal permissions using Helm.
## Requirements
- Linux kernel version: **5.5**
- Netto container run in **--privileged** mode
## Install Netto, Grafana and Pyroscope
Clone this repository
```
git clone https://github.com/pucciorenzo/netto-chart.git
cd netto-chart
```
Create a namespace for Netto in the cluster
```
kubectl create namespace netto
```
Add Grafana repository and install Pyroscope with the provided values file
```
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm -n netto install pyroscope grafana/pyroscope --values pyroscope-noagent.yml
```
Build dependencies for Grafana
```
helm dependency build
```
Finally install Netto and Grafana from the provided chart
```
helm install netto . -n netto
```
Forward Grafana on port 3000
```
kubectl port-forward -n netto service/netto-grafana 3000:80
```
## Install Netto alone
If you already have Grafana and Pyroscope configured, you can install just Netto. In this case run:
```
git clone https://github.com/pucciorenzo/netto-chart.git
cd netto-chart
```
Create a namespace for Netto in the cluster
```
kubectl create namespace netto
```
And install Netto
```
helm install netto . -n netto --set grafana.enabled=false
```
## Default configuration
By default Netto will send data to `http://pyroscope.netto.svc.cluster.local.:4040` this can be changed in `values.yml` file. 
This will also be the address used by grafana when the data source is selected.
### Credentials
Default credentials for Grafana are:
```
Username: admin
Password: admin
```
At the first login you will be asked to change password.
### Tags
Netto automatically takes the name of each host. If you need to filter data by host, in Grafana in Explore section select:
```
{service_name="netto", host="name_of_the_host"}
```
