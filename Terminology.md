# Table of Contents
1. [Overview](#overview)
2. [Controller](#controller)
3. [OpenEmpower Protocol](#openempower)
4. [Wireless Termination Points](#wtp)
5. [Click Packet Processors](#cpp)
6. [Virtual Base Stations](#vbs)

<a name="overview"/>

## Overview

The figure below sketches the 5G-EmPOWER network architecture. We name Wireless Termination Points (WTPs) the physical points of attachment in the Wi-Fi radio access networks (the Access Points), Click Packet Processors (CPPs) the forwarding nodes with packet processing capabilities and Virtual Base Stations (VBS) the physical points of attachment in the LTE radio access network (the eNodeBs).

![5G-EmPOWER Architecture](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/empower_arch.png)

**Note:** You do not need to have WTPs, CPPs, and VBSes in your network at the same time. In fact you can use just the class of devices that suits your needs. For example if you are only interested in Wi-Fi programmability you will need only WTPs in your network.

<a name="runtime"/>

## Controller

The 5G-EmPOWER Controller is responsible for the management of the heterogeneous RAN.

The 5G-EmPOWER Controller supports multiple virtual networks, or Tenants, on top of the same physical infrastructure. A Tenant is a virtual network with its own set of WTPs, VBSes, and CPPs. Network Apps run on top of the Runtime in their own slice of resources and exploit its programming primitives through either a REST API or a native Python API.

The 5G-EmPOWER Controller ensures that a Network App is only presented a view of the network corresponding to its slice.

Here follows some of the main features of the 5G-EmPOWER Controller:

**Soft State.** The only persistent information stored at the runtime are the clients authentication method (currently only ACLs are supported) and the list of network slices currently defined. All the state is kept within the network in a distributed fashion and is synchronised when the devices connect to the runtime. As a result the runtime can be hot-swapped with another instance without affecting the active clients. Moreover, the network itself can still function at its last known state even if the runtime becomes unavailable.

**Modular Architecture.** With the exception of the logging subsystem, every other task supported by the runtime is implemented as plug-in (i.e., a Python module) that can be loaded at runtime. Examples of such plug--in are the module implementing the data-path runtime protocol, the RESTful web interface and the mobility/load-balancing applications used for the demos.

**Slicing.** Multiple logical virtual networks, or slices, can be instantiated on top of the runtime. Each network is characterised by its own network name and a set of WTPs, VBSes, and/or CPPs. Applications are instantiated in one or more virtual networks and can only affect the state of the wireless clients associated to that network.

<a name="openempower"/>

## OpenEmpower protocol

Notice how in order to join an 5G-EmPOWER-managed domain, WTPs and VBSes must implement the vendor neutral [OpenEmpower](EmPOWER-Protocol) Protocol. The actual implementation of the OpenEmpower protocol is data-path-dependent. 

<a name="wtp"/>

## Wireless Termination Points (WTPs)

The WTPs are the physical devices handling the low-level communication with the clients. WTPs are essentially Wi-Fi Access Points running the 5G-EmPOWER LVAP Agent.

The picture below shows one of the WiFi WTPs used in our testbed. The WTP is based on [PCEngines Alix 2D2](http://www.pcengines.ch/alix2d2.htm) processing board and is equipped with two Mikrotic R52Hn interfaces (Atheros 802.11n with 2x2 MIMO).

![PCEngines Alix 2D WTP](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/wtp_alix_2d.jpg)

<a name="cpp"/>

## Click Packet Processors (CPPs)

The CPPs are the forwarding nodes with computational capacity. These nodes are essentially programmable switches running an embedded version of the Linux OS and capable of performing arbitrary operations on the traffic. As the name suggests the actual packet processing is performing by multiple instances of the Click Modular Router. CPPs run the 5G-EmPOWER LVNF Agent.

The picture below shows one of the x86 CPPs used in our testbed. The CPP is built upon the Soekris 6501-70 platform (1.6 GHz Intel Atom CPU, 2 Gbyte of SDRAM, and 12 Gigabit Ethernet Ports) and runs Debian 8.5 as operating system.

![Soekris 6501 CPPs](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/cpp_soekris_6501.jpg)

<a name="vbs"/>

## Virtual Base Stations (VBSes)

The VBSes are LTE eNodeBs with support for the 5G-EmPOWER eNB agent. The VBSes in our laboratory are build using [srsLTE](http://www.softwareradiosystems.com/tag/srslte/).
