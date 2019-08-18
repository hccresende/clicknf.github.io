# Table of Contents
1. [Hardware Requirements](#requiredhardware)
2. [Software Requirements](#requiredpackages)
3. [Running Controller](#runningcontroller)
4. [Configuration Process](#configuration)

<a name="requiredhardware"/>

## Hardware Requirements

One PC running a recent Linux distribution. There are no particular hardware requirements. Any reasonably recent laptop/desktop should be able to run the controller.

The reference operating system for this guide is Ubuntu 18.04

<a name="requiredpackages"/>

## Software Requirements

The following commands will update the package repository and install some dependencies:

```shell
sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install python3-pip git
```

We recommend to use pip to install all python packages:

```
sudo pip3 install netaddr eventlet oslo_config routes \
tinyrpc webob websocket-client
```

<a name="runningcontroller"/>

## Running the Controller

Checkout the Ryu Controller repository:

```
git clone https://github.com/5g-empower/empower-ryu.git
```

The Ryu Controller must be executed from a central server that can be reached from all CPPs. From the main repository enter the controller directory:

```
cd empower-ryu
```

and start the controller:

```
PYTHONPATH=./ python3 ./bin/ryu-manager --observe-links ryu.app.intent
```

If all the python dependencies are satisfied you should see an output like this:

```shell
loading app ryu.app.intent
loading app ryu.controller.ofp_handler
loading app ryu.topology.switches
loading app ryu.controller.ofp_handler
creating context wsgi
instantiating app ryu.app.intent of Intent
Trying to connect to controller ws://127.0.0.1:4444/
Socket ws://127.0.0.1:4444/ closed...
Unable to connect, trying again in 2s
instantiating app ryu.controller.ofp_handler of OFPHandler
instantiating app ryu.topology.switches of Switches
(5650) wsgi starting up on http://0.0.0.0:8080
```

<a name="configuration"/>

## Configuration Process

No configuration is needed for EmPOWER Controller which will automatically discover the Ryu controller if they both run on the same machine.
