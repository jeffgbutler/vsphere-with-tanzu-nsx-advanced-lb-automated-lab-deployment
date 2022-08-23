
https://vcsa-nsx-alb.tanzubasic.tanzuathome.net/ui

AVI Certificate:
- https://tanzu-basic-nsx-alb.tanzubasic.tanzuathome.net   (admin/VMware1!)
- Retrieve the certificate from Templates->Security->SSL/TLS Certificates


| Setting                              | Value                              |
|--------------------------------------|------------------------------------|
| **Load Balancer (AVI)**              |                                    |
| Name                                 | tanzu-basic-nsx-alb                |
| Avi Controller IP                    | 192.168.138.9:443                  |
| User Name                            | admin                              |
| Password                             | VMware1!                           |
| **Management Network**               |                                    |
| Network                              | DVPG-Supervisor-Management-Network |
| Management Network Starting IP       | 192.168.138.190                    |
| Subnet                               | 255.255.255.0                      |
| Gateway                              | 192.168.138.1                      |
| DNS                                  | 192.168.128.1                      |
| Search Domains                       | tanzubasic.tanzuathome.net         |
| NTP Server                           | pool.ntp.org                       |
| **Workload Network**                 |                                    |
| DNS                                  | 192.168.128.1                      |
| Port Group                           | DVPG-Workload-Network              |
| Gateway                              | 192.168.139.1                      |
| Subnet                               | 255.255.255.0                      |
| IP Address Ranges                    | 192.168.139.160-192.168.139.167    |

Network Notes:

- Management Network - The IP space for the supervisor cluster. (Supervisor nodes also grab IPs in the workload network - they are in both networks)
- Workload Network - The IP space for K8S nodes
- Load Balancer (Frontend) Network - IP space for service type load balancer
