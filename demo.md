# Demo

## vSphere 7 Control Plane

1. Talk about basic requirements
   - 3+ hosts with HA and DRS Enabled
   - Some load balancer (NSX-T, HAProxy, Avi)
   - This is using HAProxy. HAProxy needs 2 or 3 subnets depending on how it is configured and what kind of isolation you need (i.e. do
     front end (load balanced) IPs need to be isolated from the node IPs)
1. https://vcsa-nsx-alb.tanzubasic.tanzuathome.net/ui
1. Show Content Library (ctrl-opt-6) - this is a subscribed content library where new versions of K8S will be made available from VMware
1. Show basic workload management UI (ctrl-opt-7)
   - Cluster - a vSphere cluster where Tanzu is enabled. Creates a "supervisor cluster"
   - "Supervisor Cluster" - a K8S cluster that is primarily used for cluster lifecycle management. Used to manage namespaces and
      other cluster where workloads are placed. If, and only if, you are using NSX-T, then you can deploy workload pods on this cluster as well
   - Namespace - a namespace in the Tanzu supervisor cluster - workload clusters will be "in" this namespace. Used to set basic parameters of 
     workload domain (permissions, storage, quotas, etc.) Can also be used to delegate authority.
   - Open the namespace and show different features
   - Open the link to the CLI tools. Talk about how this could be sent to a devops team to give them instructions for how to connect
     to the namespace
1. Cluster management/creation - option 1
   - login with `kubectl vsphere login`
   - Commands to see what's possible:
      - `kubectl describe virtualmachineclasses`
      - `kubectl describe ns demo-namespace` - Storage classes
      - `kubectl get TanzuKubernetesReleases` - K8S releases
   - Create YAML for kubernetes
   - Talk a bit about security - clusters created with very little authority for the default service account
   - Talk about upgrdes/scaling (apply changes to the YAML)
1. Cluster management/creation with TMC
   - Open TMC UI
