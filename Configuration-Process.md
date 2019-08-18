# Table of Contents  
1. [Configuring the EmPOWER Agent](#empoweragent)
2. [Configuring the Controller](#controller)  
3. [Creating your Virtual network](#virtualnetwork)  

<a name="empoweragent"/>
## Configuring the EmPOWER Agent

> * To login to the WTP using serial console, refer [Minicom](https://github.com/rriggio/empower/wiki/Log-in-to-the-WRT-using-serial-console(Minicom))  
> * To enable ssh and to change firewall configurations, refer [Telnet, SSH and Firewall](https://github.com/rriggio/empower/wiki/How-to-login-to-the-WTP-through-telnet-or-ssh(Firewall-configuration)%3F)  

<a name="controller"/>
## Configuring the Controller

The first time the controller is initialized, three accounts are automatically created:

 USERNAME | PASSWORD  
 -------- | --------
 root | root  
 foo | foo  
 bar | bar  

The first account has administrator privileges, while the other two are user accounts.

Log in as root and click on the WTP tab (WTP stands for Wireless Termination Points and, in the EmPOWER terminology, is the equivalent to a Wireless AP).

You must then specify the MAC addresses of the WTPs that are authorized to connect to this controller. You can find the MAC address by logging into the WTPs using ssh(how to: [Telnet, SSH and Firewall](https://wiki.openwrt.org/doc/howto/firstlogin)) and by using the ifconfig command. The EmPOWER platform uses the MAC address of the OpenVSwitch bridge (br-ovs) as unique identified for wach WTP. Add all your WTPs using this procedure.

After all WTPs have been added and if the controller is reachable by the WTPs on the ip address specified in the "etc/config/empower" then you should see an output like this in the controller terminal:

> INFO:lvapp.lvappserver:Incoming connection from ('192.168.0.133', 54476)  
> INFO:lvapp.lvappserver:Hello from 192.168.0.133 seq 46852  
> INFO:lvapp.lvappserver:Sending caps request to 04:F0:21:09:F9:93  
> INFO:lvapp.lvappserver:Received caps response from 04:F0:21:09:F9:93  

Click on the VBSes tab (VBS stands for Virtual Base Station and, in the EmPOWER terminology, is the equivalent to a Mobile Base station).

You must then specify the VBSes identifier in the form of MAC addresses of the VBSes that are authorized to connect to this controller i.e. for example if VBS identifier is 0xe21, then its MAC address will be 00:00:00:00:0E:21. The place in which you can find the VBS identifier is mobile base station implementation specific, for example in case of OpenAirInterface, its the eNB_ID field in the configuration file. Regardless of the implementation, make sure the parameter used as VBS identifier is unique. Add all your VBSes using this procedure.

After all VBSes have been added and if the controller is reachable by the VBSes then you should see an output like this in the controller terminal:

```
INFO:core.pnfpserver:Incoming connection from ('127.0.0.1', 50593)
INFO:vbsp.vbspconnection:Hello from 127.0.0.1, VBS 00:00:00:00:0E:21
INFO:vbsp.vbspconnection:Sending eNB config request to VBSP 00:00:00:00:0E:21 (3617)
```

<a name="virtualnetwork"/>
## Creating your Virtual network

The Controller can run multiple virtual networks on top of the same physical infrastructure. A virtual network has own set of WTPs, VBSes.

Virtual Networks are requested by regular users and must be approved by a network administrator. You can request your first slice using the controller web interface by logging with one of the default user account (e.g. foo).

After logging you will be presented with the list of your Virtual Networks (which should be empty). Click on the "+" icon and then specify a name (this will be your WLAN SSID), an optional short description. Then click on the button "Request Virtual Network".

You should now logout from the web interface and login again as administrator. Click on the "Request" tab and approve the request.

You can now login again as regular user. Your new virtual network should be listed in the "Tenants" tab. Click on the virtual network id and you will be redirected to the Virtual Network management page. Here from the WTP tab you can add the WTPs to your virtual network, from VBSes tab you can add the VBS to your virtual network.

Finally, the open source version of EmPOWER does not support WPA authentication, so you must specify the list of MAC address that can connect to any of the Virtual Networks defined in the controller (by default anybody can associate). In order to do so click on the "ACL" tab and specify the clients MAC addresses that are allowed and/or denied to use the network.

At this point any of the clients that are authorized to use the network should see the new SSID and should be able to associate to it and to reach the public Internet (if the wireless APs have a backhaul that is connected to the Internet).

