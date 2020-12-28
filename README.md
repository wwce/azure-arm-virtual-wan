# Panorama Orchestration & Azure Virtual WAN
Connects Security vNets to an Azure Virtual WAN Hub to secure global inbound and cross Virtual WAN traffic through regional security vNets.

## Overview 

This build demonstrates how to secure Azure Virtual WAN traffic with VM-Series firewalls deployed through Panorama Orchestration.  The build is broken down into 3 major sections.  Certain sections may not be required if the Azure resources already exist.

### Design

The build shows two types of traffic flows through a Virtual WAN hub.  

1.  <span style="color:blue">Internet inbound traffic through a VM-Series scale set to directly connected hub virtual network (vwan-spoke)</span>
    - Additional scale sets can be added throughout different Azure regions to achieve a globally scalable inbound security edge.
2.  <span style="color:green">East-West traffic through a VM-Series scale set from a vwan-spoke to a locally peered virtual network</span>
    - This design can be adapted into larger infrastructures that have regional hub and spoke architectures.  The VM-Series in each regional hub VNET, can secure ingress traffic coming from virtual WAN hubs.
    - This can design can also be applied for traffic between ExpressRoute and VNET connections.
    - If two VNETs are connected to the same vWAN hub, it is not possible to secure lateral traffic between two vWAN hub VNET connections. 

### Prerequisites

The following items are required prior to launching the build.  

1.  An active Azure subscription with appropriate permissions and resource quota allocation.
2.  Inbound VM-Series scale set within a dedicated VNET.
3.  Outbound VM-Series scale set within a dedicated VNET.


The diagram below further illustrates the requirements before proceeding.  The greyed out resources will be built in each step of the build.

## Build Guide

#### Part 1:  Create Virtual WAN & Virtual Hub

Part 1 creates a Virtual WAN with a virtual hub.  The hub will be used in Parts 2 and 3 to direct traffic from connected spokes to the security VNETs.  If you already have a Virtual Hub, you can skip this step and proceed to part 2. 

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart1_wan.json)



#### Part 2:  Create Virtual Network & Connect to Virutal Hub

Part 2 creates a demo VNET that is directly connected to the virtual hub from part 1.  Any vNet CIDR can be used as long as it does not overlap with the VM-Series vNet CIDR blocks.   If you already have a test vNet connected to your virtual hub, you can skip this step and proceed to part 3. 

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart2_wan_spoke.json)

#### Part 3:  Connect Inbound VM-Series Scale Set to the Virtual Hub 

A dedicated virtual network with a set of VM-Series firewalls must exist prior to proceeding.  This guide uses the Panorama Orchestration inbound scale set deployment to serve as this virtual network.  

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart3_inbound.json)

#### Part 4:  Connect Outbound VM-Series Scale Set to the Virtual Hub

A dedicated virtual network must exist with a set of VM-Series firewalls prior to proceeding.  This virutal netwokr should be separate from teh virtual network used in part 3.  In this step, you will connect the existing outbound VM-Series firewalls to the virtual hub.

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart4_vnet_spoke.json)

#### Part 5:  Create Local Spoke VNET peered to outbound firewalls

 Part 4 creates a demo VNET that is peered (via vNet Peering) to the outbound firewall virtual network.  A route table is created to direct all traffic from the locally connected spoke to the outbound firewall's interal load balancer.  This will force traffic through the outbound scale set before reaching its destination. 

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart5_outbound.json)

