# Introduction

This document describes the 5G-EmPOWER IBN interface placed on top of the 
Ryu OpenFlow controller, that enables 5G-EmPOWER controller to operate on OpenFlow SDN networks. The communication between the Empower Controller and Ryu is based on websockets.

# Architecture

In the 5G-EmPOWER ecosystem, the IBN interface is consumed by the 5G-EmPOWER Controller. The IBN provides networking functionalities with a high degree of abstraction, hiding the complexity of the underlying networks. This goal is achieved by introducing these three concepts:

* Endpoint
* Virtual Port
* Virtual Link

An Endpoint is a host / termination point in the network. An Endpoint that has one or more  Virtual Ports, where each Virtual Port represents one physical port.

A Virtual Port contains some information about the corresponding physical port, in particular, the most relevant ones are:
* The OpenFlow datapath id to which the port is attached
* The port number on that datapath id

The Endpoints with their Virtual Ports can be used for declaring Virtual Links, where each of them is composed by:
* Source Endpoint
* Source Virtual Port (from the same source Endpoint)
* Destination Endpoint
* Destination Virtual Port (from the same destination Endpoint)

Additionally, the following parameters can be specified:
* OpenFlow match, for selecting the traffic that has to be forwarded through the Virtual Link
* Actions, in case the Virtual Link has to perform some actions on the traffic packets
* Priority, for specifying different priorities among Virtual Links

No additional data is required, since the Virtual Ports already have all
the necessary information for creating a Physical Link.

The image below shows two Virtual Links deployed on an OpenFlow network, and
their respective Physical Links.

![IBN ARCHITECTURE](https://github.com/5g-empower/5g-empower.github.io/blob/master/img/IBN.png)

In order to facilitate the Endpoints and Virtual Links declaration, the IBN sends to the 5G-EmPOWER Controller
the network topology, that is composed by:
* Datapaths (switches)
* Links
* Hosts

# Run Ryu with the 5G-EmPOWER IBN

The IBN has been developed as a Ryu app that can be launched from the command line:

``` shell 
empower-ryu/bin/ryu-manager --observe-links ryu/app/intent.py --config-file <config file>
```

The app configuration file contains the ip address and the port of the 5G-EmPOWER Controller.

```
[DEFAULT]
empower_ip = <ip address>
empower_port = <port>
```

In case no configuration is provided, the app is run with the default values (127.0.0.1 - 4444)

# IBN Protocol

This document provides the full IBNP reference for the Empower Ryu IBN

## Common

Each message contains the following fields, in addition to the IBN element specific ones

* version: protocol version (currently it is 0)
* type: message type identifier, each IBN element has one
* seq: message sequence number
* every: (only from Empower Ryu to Empower Controller), the period between hellos in seconds

Example:

```
{
  "version": 0,
  "type": "hello",
  "seq": 1,
  "every": 2
}
```

## Endpoints

### Add / Update endpoint

Direction: Empower Controller -> Empower Ryu

Message Type: update_endpoint

Parameters:

* uuid: endpoint identifier, it has to be provided to Empower Ryu and should be
saved for future endpoint update and deletion operations
* dpid: the datapath id of the endpoint
* ports: a dictionary of Virtual Ports, where the keys are the virtual port identifiers
and the values are dictionaries containing the following data
  * port_no: the port number on the datapath id
  * properties: a dictionary of additional information
    * dont_learn: a list of MAC addresses that will be bounded to the parent dpid-port,
    preventing the default learning switch behavior for those hosts

Example:

```
{
  "uuid": "714568ea-54e7-47a6-bc8e-f9a7d4e28f3a",
  "dpid": "00:00:00:0D:B9:2F:56:B4",
  "ports": {
    "0": {
      "port_no": 1,
      "properties": {
        "dont_learn": [
          "00:24:D7:7B:9B:D8",
          "00:24:D7:35:06:18"
        ]
      }
    },
    "1": {
      "port_no": 2,
      "properties": {
        "dont_learn": []
      }
    }
  }
}
```

### Delete endpoint

Direction: Empower Controller -> Empower Ryu

Message Type: remove_endpoint

Parameters:

* uuid: the endpoint identifier

Example:

```
{
  "uuid": "714568ea-54e7-47a6-bc8e-f9a7d4e28f3a"
}
```

## Rules (Virtual Links)

### Add Virtual Link

Direction: Empower Controller -> Empower Ryu

Message Type: add_rule

Parameters:

* uuid: rule identifier, it has to be provided to Empower Ryu and should be
saved for deleting it in future
* stp_uuid: the source endpoint uuid
* stp_vport: the source endpoint virtual port identifier
* ttp_uuid: the destination endpoint uuid
* ttp_vport: the destination endpoint virtual port identifier
* match (optional field): a dictionary of OpenFlow matches (WARNING: the "in_port" condition
must not be provided, as it is appended by the IBN to match the physical
port of the corresponding endpoint virtual port). The default value is an empty list
* actions (optional field): a list of dictionaries of OpenFlows actions; similarly to the match, the output port
must not be provided since it is obtained by IBN through the shortest path algorithm.
Each action contains the following data, where the key(s) between quotes may be
present or not depending on the action type; for further information on the supported
actions please check [the ryu OpenFlow library.](https://github.com/osrg/ryu/blob/master/ryu/lib/ofctl_v1_0.py)
by default this field is an empty list:
  * type: the action identifier
  * "value type name": the action value
* priority (optional field): the rule priority, the allowed values are in the range [0, 49], which is mapped
into the OpenFlow [250, 299] priority range. The default value is 0


Example:

```
{
  "uuid": "2a995fa7-f33b-4a81-83f4-81dc04294d68",
  "stp_uuid": "714568ea-54e7-47a6-bc8e-f9a7d4e28f3a",
  "stp_vport": 0,
  "ttp_uuid": "714568ea-54e7-47a6-bc8e-f9a7d4e28f3a",
  "ttp_vport": 1,
  "match": {
    "tp_dst": 1234,
    "dl_dst": "00:24:d7:7b:9b:d8",
    "dl_type": 2048,
    "nw_proto": 17
  },
  "actions": [
    {
      "type": "SET_NW_TOS",
      "nw_tos": "0xC0"
    }
  ],
  "priority": 9
}
```

### Delete Virtual Link

Direction: Empower Controller -> Empower Ryu

Message Type: remove_rule

Parameters:

* uuid: the rule identifier

Example:

```
{
  "uuid": "2a995fa7-f33b-4a81-83f4-81dc04294d68"
}
```

## Hello

Direction: Empower Ryu -> Empower Controller

Message Type: hello

Empty keepalive message that is periodically sent by the Ryu IBN to the controller.

## Network elements advertisement

### New Datapath

Direction: Empower Ryu -> Empower Controller

Message Type: new_datapath

Parameters:

* dpid: the datapath id
* ip_addr: the switch ip address
* ports: a list of dictionaries, where each entry represents a network port
described as follows:
  * name: the port/interface name
  * port_no: the port number
  * hw_addr: the port/interface MAC address
  * dpid: the parent datapath id

Example:

```
{
  "dpid": "00:00:00:0D:B9:2F:56:B4",
  "ip_addr": "192.168.0.154",
  "ports": [
    {
      "name": "eth0",
      "port_no": 1,
      "hw_addr": "00:0d:b9:2f:56:b4",
      "dpid": "00:00:00:0D:B9:2F:56:B4"
    },
    {
      "name": "empower0",
      "port_no": 2,
      "hw_addr": "b6:33:8d:86:19:e9",
      "dpid": "00:00:00:0D:B9:2F:56:B4"
    }
  ]
}
```

### New Link

Direction: Empower Ryu -> Empower Controller

Message Type: new_link

Parameters:

* src: a dictionary representing the link source network port, described as below:
  * name: the port/interface name
  * port_no: the port number
  * hw_addr: the port/interface MAC address
  * dpid: the parent datapath id
* dst: a dictionary representing the link destination network port, described as above

Example:

```
{
  "src": {
    "name": "ge-1/1/47",
    "port_no": 47,
    "hw_addr": "c4:54:44:4f:2b:2e",
    "dpid": "5E:3E:C4:54:44:4F:2B:2E"
  },
  "dst": {
    "name": "ge-1/1/48",
    "port_no": 48,
    "hw_addr": "c4:54:44:4f:2b:ce",
    "dpid": "5E:3E:C4:54:44:4F:2B:CE"
  }
}
```

### New Host

Direction: Empower Ryu -> Empower Controller

Message Type: new_host

Parameters:

* mac: the host mac address
* port: a dictionary representing the network port to which the host has been
discovered, described as below
  * name: the port/interface name
  * port_no: the port number
  * hw_addr: the port/interface MAC address
  * dpid: the parent datapath id
* ipv4: a list containing the host ipv4 addresses (usually only one value is reported)
* ipv6: a list containing the host ipv6 addresses (usually only one value is reported)

Example:

```
{
  "mac": "00:24:d7:7b:9b:d8",
  "port": {
    "dpid": "00:00:00:0D:B9:2F:56:B4",
    "port_no": 2,
    "name": "empower0",
    "hw_addr": "b6:33:8d:86:19:e9"
  },
  "ipv4": [
    "192.168.0.172"
  ],
  "ipv6": []
}
```
