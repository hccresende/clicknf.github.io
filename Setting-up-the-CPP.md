# Table of Contents  
1. [Hardware Requirements](#requiredhardware)  
2. [Software Requirements](#requiredpackages)  
3. [Compilation Process](#compilation)  
4. [Configuration Process](#configuration)
5. [Starting the CPP](#starting)

<a name="requiredpackages"/>

## Hardware Requirements

One or more x86 (or x86-64) machines running a recent Linux distribution and supporting OpenVSwitch (2.3 or higher) with at least two Ethernet ports.

The reference operating system for this guide is Ubuntu 18.04.

<a name="requiredpackages"/>

## Software Requirements

The following commands will update the package repository and install some dependencies:

``` shell
sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install build-essential openvswitch-switch python3-pip git
```

We recommend to use pip to install all the required python packages:

``` shell
sudo pip3 install websocket-client
```

<a name="compilation"/>

## Compilation Process

Clone the empower-lvnf-agent git repository (this will provide the actual EmPOWER LVNF Agent):  

``` shell
git clone https://github.com/5g-empower/empower-lvnf-agent.git
```

Clone the empower-lvap-agent git repository (this will provide the click executable and the LVNFs):  

``` shell
git clone https://github.com/5g-empower/empower-lvap-agent.git
```

Compile and install click:  

``` shell
cd empower-lvap-agent
./configure --disable-linuxmodule --enable-userlevel --enable-wifi --enable-empower --enable-lvnfs 
make 
sudo make install
```

<a name="configuration"/>

## Configuration Process

You now need to configure the system to use OpenVSwitch. 

Create a new OpenVSwitch bridge:
  
``` shell
sudo ovs-vsctl add-br br0
```

Then add the ports to the new bridge:

``` shell
sudo ovs-vsctl add-port br0 en2
```

Link the executable:

``` shell
sudo ln -s ~/empower-lvnf-agent/empower-lvnf-agent.py /usr/bin/empower-lvnf-agent
```

Link the init script:

``` shell
sudo ln -s ~/empower-lvnf-agent/scripts/empower-lvnf-agent /etc/init.d/
```

Link the rc.local script:

``` shell
sudo ln -s ~/empower-lvnf-agent/scripts/rc.local /etc/
```

<a name="starting"/>

## Starting the CPP

At this point you can start the LVNF Agent with:

``` shell
sudo /etc/init.d/empower-lvnf-agent restart
```
