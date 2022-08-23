# Installing TKGM on vSphere

Requires docker on the installing machine!
Make a resource pool TKGM-RP on the cluster


## Downloads

https://my.vmware.com/en/web/vmware/downloads/info/slug/infrastructure_operations_management/vmware_tanzu_kubernetes_grid/1_x

- Download the NSX Advanced Load Balancer
- Download photon-3-kube-v1.20.4 ... .ova


## Network Details

"Management" Network - 192.168.128.0/24 - where NSX ALB Lives
Workload Network - 192.168.133.0/24
   - DHCP: 192.168.133.10-192.168.133.100
   - "Workload" Network: 192.168.133.101-192.168.133.199
   - "Frontend" Network: 192.168.133.200-192.168.133.250

## Install NSX ALB

https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.3/vmware-tanzu-kubernetes-grid-13/GUID-mgmt-clusters-install-nsx-adv-lb.html

IP is 192.168.128.132/24 in the main management network.

Set aside 192.168.133.200-192.168.133.250 as the VIP pool (Infrastructure->Networks)

## Install TKG

1. Deploy OVA template photon... OVA, set network vm-network-133. I picked thin disk provisioning
1. Convert the OVA to a template - do not power on!

1. `tanzu management-cluster create --ui`
1. Set control plane endpoint to 192.168.133.101
1. NSX ALB Settings:
   - Host: 192.168.128.132
   - Cloud: Default-Cloud
   - Service Engine Group: Default-Group
   - VIP network name: vm-network-133
   - VIP Network CIDR: 192.168.133.0/24
   - Controller CA: Obtain from AVI (Templates->Security->SSL/TLS Certificates)
1. Resources:
   - VM Folder: TKGM
   - Datastore: VMStorage
   - Cluster: TKGM-RP
1. Network:
   - Netowrk Name: vm-network-133
   - Cluster Service CIDR: 100.64.0.0/13 (Default)
   - Cluster Pod CIDR: 100.96.0.0/11 (Default)
1. Disable Identity Management
1. Pick the OS Image
1. Don't Register with TMC

Took about 12 minutes

## Create Cluster

For configuration, copy the default config file from ~/.tanzu/tkg/clusterconfigs, change the IP address and the name, then

`tanzu cluster create -f <<filename>>`

Information about the cluster:
`tanzu cluster get tkgm-test-cluster`

Create a K8S YAML for new clusters:
`tanzu cluster create -f <<filename>> -d > cluster.yaml`

Show clusters:
`tanzu cluster list --include-management-cluster`

Get credentials for a new cluster:
`tanzu cluster kubeconfig get tkgm-test-cluster --admin`

## Tear Down

`kubectl delete service my-loadbalanced-service`
`kubectl delete deployment nginx-deployment`
Switch to management cluster
`tanzu cluster delete tkgm-test-cluster`

