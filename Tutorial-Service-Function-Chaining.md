# Table of Contents  
1. [Objective](#objective)  
2. [Pre-requisites](#prereq)  
3. [Launch method & Initializations Tasks](#init) 
4. [Deploy the LVNFs](#deploy)  
5. [Service Function Chaining](#chain)  

<a name="objective"/>

## Objective

In this section we shall illustrate how to implement deploy a single LVNF and how to steer the HTTP traffic generated by the wireless clients through it.

The complete implementation of the Service Function Chaining Network application discussed in this tutorial can be found [here](https://github.com/5g-empower/empower-runtime/blob/master/empower/apps/lvnfchain/lvnfchain.py).

<a name="prereq"/>

## Pre-requisites

This tutorial requires a network setup similar to the one described [here](https://github.com/5g-empower/5g-empower.github.io/wiki/Network-setup#wtpscpps). This means at least one WTP connected to a CPP. Moreover both the 5G-EmPOWER Controller and the Ryu Controller must be active.

Refer to [this](https://github.com/5g-empower/5g-empower.github.io/wiki/Setting-up-the-Backhaul-Controller) page for instructions about how to setup the Ryu controller. If any other switch is present between the WTP and the CPP that switch must be an OpenFlow switch and must point to the Ryu controller. 

<a name="init"/>

## Launch method & Initializations Tasks

The Service Function Chaining class definition and initialization methods are reported below:

```python
class LvnfChain(EmpowerApp):
    def __init__(self, **kwargs):
        EmpowerApp.__init__(self, **kwargs)
        self.cppup(callback=self.cpp_up_callback)
        self.lvapjoin(callback=self.lvap_join_callback)
        self.lvnfjoin(callback=self.lvnf_join_callback)
        self.lvnf = None
```

As it can be seen, the initialization tasks registers three triggers: _lvapjoin_, _lvnfjoin_, and _cppup_.

The launch method for the Mobility Manager application is shown below:

``` python
def launch(tenant_id, every=DEFAULT_PERIOD):
    """ Initialize the module. """

    return LvnfChain(tenant_id=tenant_id, every=every)
```

As it can be seen the _launch_ method has two parameters: the mandatory _tenant_id_ parameter, and the _every_ parameter specifying the control task period.

<a name="deploy"/>

## Deploy the LVNFs

In this case the _cppup_ trigger is used to deploy an LVNF when a CPP becomes available. The _cppup_ callback method is reported below:

```python
def cpp_up_callback(self, cpp):
    img = Image(vnf="in_0 -> ToDump(/tmp/lvnf.pcap) -> out_0")
    self.spawn_lvnf(img, cpp)
```

Notice how this LVNF essentially consists of a single Click element which simply saves all the traffic to a file.

The _lvnf_join_callback_ method is reported below. This method is used in order to save a pointer th LVNF as soon as it becomes active.

```python
def lvnf_join_callback(self, lvnf):
    self.lvnf = lvnf
```

<a name="chain"/>

## Service Function Chaining

Finally, the _lvap_join_callback_ method implements the service function chaining between any new wireless client and the LVNF:

```python
def lvap_join_callback(self, lvap):
    if self.lvnf:
        lvap.ports[0].next["tp_dst=80"] = self.lvnf.ports[0]
```

At this point if you login using SSH into the CPP running the LVNF you will find a file named _/tmp/lvnf.pap_. You can either open this file using tcpdumpp (if this software has been installed in the CPP) or you can copy it to your laptop and open it with a software like Wireshark. Inside the file you will find only the HTTP requests made by the client.

The LVNF can then be modified in order to run you own Click element implementing a more complex function, e.g. filtering out some HTTP requests.