# Panorama Orchestration & Azure Virtual WAN
Secure Azure Virtual WAN traffic with Palo Alto Networks VM-Series firewalls. 

## Overview 

This build illustrates how to secure Azure Virtual WAN traffic with VM-Series scale sets. The build is broken down into 5 Parts.  Depending on existing Azure resources, certain parts may not be required.

- **Part 1.** Create Virtual WAN & Virtual Hub </br>
- **Part 2.** Connect VM-Series Inbound Scale Set to the Virtual Hub </br>
- **Part 3.** Connect VM-Series Outbound Scale Set to the Virtual Hub </br>
- **Part 4.** Create Local Spoke VNET </br>
- **Part 5.** Create Virtual Hub Spoke VNET </br>


### Architecture

The build shows two types of traffic flows through a Virtual WAN hub.  

1.  <span style="color:blue">Internet inbound traffic through a VM-Series scale set to directly connected hub virtual network (vwan-spoke)</span>
    - Additional scale sets can be added throughout different Azure regions to achieve a globally scalable inbound security edge.
2.  <span style="color:green">East-West traffic through a VM-Series scale set from a vwan-spoke to a locally peered virtual network</span>
    - This design can be integrated into larger infrastructures that have regional hub and spoke architectures.  The VM-Series in each regional hub VNET, can secure ingress traffic coming from virtual WAN hubs.
    - This can design can also be applied for traffic between ExpressRoute and VNET connections.
    - If two VNETs are connected to the same vWAN hub, it is not possible to secure lateral traffic between two vWAN hub VNET connections. 


<p align="center">
<img src="https://raw.githubusercontent.com/wwce/azure-arm-virtual-wan/main/images/overview.png" alt="drawing" width="800"/>
</p>

### Prerequisites

The following items are required prior to launching the build.  

1.  A active Azure subscription with appropriate permissions and resource allocation quota.
2.  Panorama
    - PAN-OS 10.0 or greater
    - Azure Plugin 3.0 or greater
3.  Inbound & Outbound VM-Series scale set within a dedicated VNETs.

**If you do not have the VM-Series deployed, please see [Deployment Guide](https://github.com/wwce/azure-arm-virtual-wan/main/GUIDE.pdf) for how-to.**

</br>

##  Guide

#### <img align="right" width="400" src="https://raw.githubusercontent.com/wwce/azure-arm-virtual-wan/main/images/part1.png"> Part 1:  Create Virtual WAN & Virtual Hub

In this part, a Virtual WAN is created with a virtual hub.  The hub will be used in Parts 2 and 3 to direct traffic from connected spokes to the security VNETs.  If you already have a Virtual Hub, you can skip this step and proceed to part 2. 
</br>
</br>
</br>
</br>
</br>
</br>
</br>
[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart1_wan.json)
</br>
</br>
</br>

#### <img align="right" width="400" src="https://raw.githubusercontent.com/wwce/azure-arm-virtual-wan/main/images/part2.png"> Part 2:  Connect Inbound VM-Series Scale Set to the Virtual Hub 
Connects an existing VNET that contains a dedicated inbound set of VM-Series firewalls to the virtual hub created in part 1.
</br>
</br>
</br>
</br>
</br>
</br>
</br>
</br>
[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart2_connect_inbound.json)
</br>
</br>
</br>

#### <img align="right" width="400" src="https://raw.githubusercontent.com/wwce/azure-arm-virtual-wan/main/images/part3.png"> Part 3:  Connect Outbound VM-Series Scale Set to the Virtual Hub
Connects an existing VNET that contains a dedicated outbound set of VM-Series firewalls to the virtual hub created in Part 1.
</br>
</br>
</br>
</br>
</br>
</br>
</br>
[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart3_connect_outbound.json)
</br>
</br>
</br>

#### <img align="right" width="400" src="https://raw.githubusercontent.com/wwce/azure-arm-virtual-wan/main/images/part4.png"> Part 4:  Create Local Spoke VNET
Creates a spoke VNET that is peered (via vNet Peering) to the outbound VM-Series VNET.  A route table is created to direct all traffic from the spoke VNET to the outbound firewall's interal load balancer.  A route is also added to the virtual hub's route table.  This route will direct virtual WAN traffic destinted to the spoke VNET through the outbound VM-Series VNET connection that was created in part 3.   
</br>
</br>
</br>
[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart4_create_peer_spoke.json)
</br>
</br>
</br>

#### <img align="right" width="400" src="https://raw.githubusercontent.com/wwce/azure-arm-virtual-wan/main/images/part5.png"> Part 5:  Create Virtual Hub Spoke VNET
Creates a spoke VNET that is directly connected to the virtual hub created in part 1.  This VNET is used to demonstrate/test internet inbound traffic through the inbound VM-Series and also lateral traffic through the outbound VM-Series. 
</br>
</br>
</br>
</br>
</br>
</br>
[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart5_create_vhub_spoke.json)

