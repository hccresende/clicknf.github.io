# Table of Contents  
1. [Hardware Requirements](#hardware)  
2. [Software Requirements](#software)  

<a name="hardware"/>
## Hardware Requirements

For the controller:  
* One server-class machine running a recent Linux distribution.    

For the WTPs:  
* One or more WiFi Access Points capable of running OpenWRT Barrier Breaker (15.05) with at least one Atheros-based Wireless NIC supported by the ath9k driver.

For the CPPs:
* One or more embedded PC running a recent linux distribution and supporting OpenVSwitch with at least two Ethernet ports.

For the VBSes:
* VBS: A Quad core PC (i5 or better) with 8 or 16 GB RAM and running Ubuntu 14.04 LTS with the 3.19.8-031908-lowlatency kernel. A Software Defined Radio platform (e.g. USRP B210 (UHD ver. 3.10.0).

Notice that you do not need to have WTPs, CPPs, and VBSes in your network at the same time. In fact you can use just the class of devices that suits your needs. For example if you are only interested in Wi-Fi programmability you will need only WTPs in your network.

<a name="software"/>
## Software Requirements

All the software packages and supported versions that are required are mentioned below:  
* For the WTPs: [Check here](Setting-up-the-WTP#requiredpackages).  
* For the CPPs: [Check here](Setting-up-the-CPP#requiredpackages).  
* For the Controller: [Check here](Setting-up-the-Controller#requiredpackages).  
* For the VBSes: [Check here](Setting-up-the-VBS-OAI#requiredpackages).  
