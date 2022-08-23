# Implementation Notes

## Edge Router/VLAN Configuration

| Port Group Name | VLAN ID | Subnet           |
|-----------------|---------|------------------|
| vm-network-138  | 138     | 192.168.138.0/24 |
| vm-network-139  | 139     | 192.168.139.0/24 |

The VLANs need to be trunked together.

In the outer port group for each VLAN associated with this install (138 and 139), edit the settings to
accept promiscuous mode and forged transmits.

Make sure that DNS wil listen to the VLANs!

## Management Network (192.168.138.1/24)

3: vcsa
4: esxi1
5: esxi2
6: esxi3
9: nsx-alb
128-192: NSX ALB Service Engines
193-254: Supervisor Control Plane

## Workload/Load Balancer Network (192.168.139.1/24)

Workload Network IP Range: 2 - 127
LB Range: 128 - 191


## DNS Entries

| Name                                                  | Address         |
|-------------------------------------------------------|-----------------|
| vcsa-nsx-alb.tanzubasic.tanzuathome.net               | 192.168.138.3   |
| tanzu-basic-nsx-alb-esxi-1.tanzubasic.tanzuathome.net | 192.168.138.4   |
| tanzu-basic-nsx-alb-esxi-2.tanzubasic.tanzuathome.net | 192.168.138.5   |
| tanzu-basic-nsx-alb-esxi-3.tanzubasic.tanzuathome.net | 192.168.138.6   |
| tanzu-basic-nsx-alb.tanzubasic.tanzuathome.net        | 192.168.138.9   |

## VM Setup

Increased the size of the disk and memory of the nested ESXI VMs. As it stood, there was very little room left in the VSAN.

## HAProxy Load Balancer Range

William's script has the load balancer range overlapping the gateway on the workload netowrk. This caused issues in my setup.

I changed the range to 192.168.137.64/26 (192.168.137.64-192.168.137.127)

## HAProxy CA Cert

You will need the CA cert from HA proxy. Here's how to get it:

- `ssh root@haproxy.tanzubasic.tanzuathome.net`
- `cat /etc/haproxy/ca.crt`

If you have rebuilt the envirnment, you might get an error from SSH about the host key changing. If that happens, remove the prior key like this:

`ssh-keygen -R haproxy.tanzubasic.tanzuathome.net`

## Mac Gatekeeper

When you download the VCSA ISO, it will be blocked from running by Mac Gatekeeper. To turn this off:

`sudo xattr -r -d com.apple.quarantine VMware-VCSA-all-7.0.1-17491101`


## Other

There will be a warning about JSON being truncated while running the script - you can ignore this warning.

## Install Times

1. Initial running of the script: about 45 minutes
1. Sync the content library: about 30 minutes
1. Enable workload management: about 45 minutes
