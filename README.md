# Panorama Orchestration & Azure Virtual WAN Build
Connects Security vNets to an Azure Virtual WAN Hub to secure global inbound and cross Virtual WAN traffic through regional security vNets.

## Overview This build demonstrates how to secure Azure Virtual WAN traffic with VM-Series firewalls deployed through Panorama Orchestration.  The build is broken down into 3 major sections.  Please note, not all sections are required if the resources already exist.

## Part 1:  Create Virtual WAN & Virtual Hub

Part 1 creates a Virtual WAN with a virtual hub.  The hub will be used in Parts 2 and 3 to direct traffic from connected spokes to the security VNETs.  If you already have a Virtual Hub, you can skip this step and proceed to part 2. 


[<img src="http://azuredeploy.net/deploybutton.png"/>](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart1_wan.json)

## Part 2:  Create Virtual Network & Connect to Virutal Hub

Part 2 creates a demo VNET that is directly connected to the virtual hub from part 1.  Any vNet CIDR can be used as long as it does not overlap with the VM-Series vNet CIDR blocks.   If you already have a test vNet connected to your virtual hub, you can skip this step and proceed to part 3. 

[<img src="http://azuredeploy.net/deploybutton.png"/>](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart2_wan_spoke.json)

## Part 3:  Connect Inbound VM-Series Scale Set to the Virtual Hub 

Before you proceed, a dedicated virtual network with a set of VM-Series firewalls must exist prior to proceeding.  This guide uses the Panorama Orchestration inbound scale set deployment to serve as this virtual network.  

[<img src="http://azuredeploy.net/deploybutton.png"/>](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fwwce%2Fazure-virtual-wan-connect%2Fmain%2Fpart3_inbound.json)