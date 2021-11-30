# kubernetes-cluster
Kubernetes cluster deployment with ansible and provisioned with terraform for an nginx deployment with its service and ingress setup

## Architecture
This project contains the setup for a kubernetes HA cluster
No security concern added yet

The roles in the ansible directory are splitted
- Common role is for all nodes and masters of the kubernetes cluster it contains the minimum need
- Leader contains the configuration and setup for the master acting as leader of the cluster
- Followers contains the joining process of other masters
- Workers contains the joining process of nodes
- Proxy contains the setup of haproxy as a load balancer for our api-server and a load-balancer for the ingress controller

The terraform part was just a test using the kubernetes provider
It deploys an nginx deployment with 3 replicas and deploy the ralated service
I've got an error setting up the nginx-ingress service
> Error: Failed to create Ingress 'default/nginx-ingress' because: the server could not find the requested resource (post ingresses.extensions)
    │ 
    │   with kubernetes_ingress.nginx-ingress,
    │   on main.tf line 58, in resource "kubernetes_ingress" "nginx-ingress":
    │   58: resource "kubernetes_ingress" "nginx-ingress" {

The ingress service is specified in a yaml file inside ingress directory

## Setup
```
ansible-playbook -i hosts.yml proxy.yml
ansible-playbook -i hosts.yml cluster.yml
terraform apply
```

Then execute the content of patch.sh to set the external IPS of your ingress controller service. Adapt it to your architecture