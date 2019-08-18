# Table of Contents  
1. [Hardware Requirements](#requiredhardware)  
2. [Software Requirements](#requiredpackages)  
3. [Compilation/Configuration Process](#compilationconfiguration)  
4. [Flashing the image](#flashingimage)  
5. [Configuring the WTP](#config)  

<a name="requiredhardware" />

## Hardware Requirements 

One or more WiFi Access Points capable of running LEDE 17.01 with at least one Atheros-based Wireless NIC supported by the ath9k driver.

One PC running a recent Linux distribution will be need to actually build the firmware. There are no particular hardware requirements. Any reasonably recent laptop/desktop should be able to build the firmware.

The reference operating system for this guide is Ubuntu 18.04.

<a name="requiredpackages"/>

## Software Requirements

These are the requirements for the machine you will use to build thefirmware. Make sure to install the following packages before going ahead further.

```
sudo apt-get update
sudo apt-get install build-essential libncurses5-dev zlib1g-dev libssl-dev gawk \
subversion unzip git python2.7
```

<a name="compilationconfiguration"/>

## Compilation/Configuration Process

Clone the EmPOWER LEDE repository:  

```
git clone https://github.com/5g-empower/empower-lede.git 
```

Change to the "empower-lede" directory, update, and install all feeds:  

```
cd empower-lede 
./scripts/feeds update -a  
./scripts/feeds install -a  
```

EmPOWER WTPs need to be connected to the Controller for all their operations. Therefore it is recommended to reserve an IP address for the Controller on the DHCP server. You can refer to the documentation of your router in order to learn how to reserve static IP addresses.

In order to avoid having to configure manually all access points, you can automatically create an image with the configuration that suits your needs (note, the following configuration is for a PCEngines ALIX 2D board equipped with a single Wireless NIC). In order to do so, create a directory named "files":

```
mkdir files
```

The LEDE buildroot will copy all the contents of the "files" directory to the files image.

Create a "etc/config" directory:  

```
mkdir -p files/etc/config  
```

Then create the EmPOWER configuration file:

```
touch files/etc/config/empower
```

Open the file with your preferred text editor and copy the following text (make sure to replace 127.0.0.1 with the ip address at which the empower-runtime is reachable.)

``` 
 config empower general  
     option "master_ip" "127.0.0.1"
     option "master_port" "4433"
     option "network" "wan"  
     list "ifname" "empower0" 
```

The network configuration file:

```
touch files/etc/config/network
```

Open the file with your preferred text editor and copy the following text:

``` 
 config interface 'loopback'  
     option ifname 'lo'  
     option proto 'static'  
     option ipaddr '127.0.0.1'  
     option netmask '255.0.0.0'  

 config device  
     option name 'br-ovs'  
     option type 'ovs'  
     list ifname 'eth0'  

 config interface 'wan'  
     option ifname 'br-ovs'  
     option proto 'dhcp'  

 config interface lan  
     option ifname   eth1  
     option proto    static  
     option ipaddr   192.168.250.1  
     option netmask  255.255.255.0  
```

Finally, create the wireless configuration file:

```
touch files/etc/config/wireless
```

If you want to use just 11a/b/g mode you can use the following configuration:

``` 
config wifi-device  radio0
        option type     mac80211
        option channel  36
        option hwmode   11a
        option path     'pci0000:00/0000:00:0c.0'

config wifi-iface empower0
        option device   radio0
        option mode     monitor
        option ifname   moni0
```

Otherwise if you want to use 11n mode you can use the following configuration:

``` 
config wifi-device  radio0
        option type     mac80211
        option channel  36
        option hwmode   11n
        option htmode   HT20
        option path     'pci0000:00/0000:00:0c.0'

config wifi-iface empower0
        option device   radio0
        option mode     monitor
        option ifname   moni0
```


Run the configuration application:  

```
make menuconfig 
```

From the menu select the Target System (e.g. x86) and the Subtarget (e.g. AMD Geode based systems). Then select "Network -> empower-lvap-yagent" and "Network -> openvswitch". You may also want to compile the LuCI web interface by selecting "LuCI -> Collections -> luci".

Save the configuration and exit, then start the compilation. If you have a multi core machine you can increase the compilation speed by increasing the number of parallel builds with the "-j" option.  

```
make -j 3
```

<a name="flashingimage"/>

## Flashing the image

Once the compilation is done, the compiled image can be found in the bin/ directory. For example in the case of PCEngines Alix platform, the image will be:  

```
bin/targets/x86/geode/lede-x86-geode-combined-squashfs.img
```

The method of loading the image on the wireless router (flashing) changes according to the brand/model of the wireless router. You can find the flashing instruction for all the supported models [here](https://wiki.openwrt.org/toh/start).

In the case of the PCEngines Alix board you need to insert the Compact Flash card in a compact flash reader attached to your laptop and then run:  

```
dd if=./bin/targets/x86/geode/lede-x86-geode-combined-squashfs.img of=/dev/sdb 
```

Note this assumes that the compact flash device is "/dev/sdb".

Insert the compact flash card in your router and power it up.

<a name="config"/>

## Configuring the WTP

No special configuration is needed on the WTP. If everything was configured properly in the previous steps and if the WTP has IP connectivity to the controller everything should work out-of-the-box.

However, if you need to log into the WTP you may need to make a few changes to the firewall configuration in order to open port 22 on the WAN interface (the Ethernet port next to the power plug).

If you attach a laptop to the second ethernet port you will receive automatically an ip address. Open a browser and point it to `http://192.168.250.1`.

After you login you can refer to the official OpenWRT wiki for information about how to open the SSH port on the WAN interface. Anyway the web interface is pretty intuitive so it should be a big deal to navigate the various menu to find the firewall configuration tab.
