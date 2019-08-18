# Table of Contents
1. [WTPs Only](#wtps)
2. [WTPs and CPPs](#wtpscpps)
3. [VBSes Only](#vbses)

All the network setups described in this section have a few common elements, namely one legacy switch, and one Router. The router is assumed to run DHCP/DNS servers and to perform NAT.

A simple home router can easily be used for this task. Likewise the switch can be a consumer switch with at least 4 Ethernet ports (many home routers often have an integrated switch).

It is recommended that all nodes in your EmPOWER managed network (Runtime, WTPs, CPPs, and VBSes) are in the same sub-network. It is also recommended to either use a static IP address for the Runtime node or to reserve one IP address for the Runtime on the DHCP server.

<a name="wtps"/>

## WTPs Only

The figure below sketches a very simple network setup consisting of two WTPs. In this setup the WTPs and the Wi-Fi clients will receive their IP address from the DHCP server. Instructions about how to setup the WTPs can be found [here](Setting-up-the-WTP).

![Simple Network Setup with just WTPs](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/SimpleNetwork.png)

<a name="wtpscpps"/>

## WTPs and CPPs

The figure below sketches a network setup consisting of two WTPs and one CPP.

Notice how the CPP has two connections to the switch for a total of four Ethernet interfaces. The first one is configured to receive the IP address from a DHCP server. If you configured the CPP following the instructions found [here](Setting-up-the-CPP) then this interface will be used as out-of-band OpenFlow signaling interface. The other interfaces are bridged together using OpenVSwitch.

Finally, notice how in this setup also an OpenFlow controller is present. This controller must implement the Intent based networking interface necessary for Service Function Chaining. Instructions for setting up this controller can be found [here](https://github.com/5g-empower/5g-empower.github.io/wiki/Setting-up-the-Controller-Ryu).

As for the previous example, the CPPs, the WTPs, and the Wi-Fi clients will receive their IP address from the DHCP server.

![Simple Network Setup with WTPs and CPPs](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/SimpleNetworkCPP.png)

<a name="vbses"/>

## VBSes Only

The figure below sketches a network setup consisting of one VBS.

The VBS consists of one laptop running the OpenAirInterface eNB, one laptop running running the OpenAirInterface EPC, and one USRP B210 Software Defined Radio.

![Simple Network Setup with WTPs and VBSes](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/SimpleNetworkVBS.png)

