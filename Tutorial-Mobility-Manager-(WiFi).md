# Table of Contents  
1. [Objective](#objective)  
2. [Pre-requisites](#prereq)  
3. [Launch method ](#init)  
4. [Handover](#handover)

<a name="objective"/>

## Objective

In this section we shall illustrate how to implement a simple mobility management application. This application is required to:

* Measures the link quality experienced by an LVAP and, if the link quality deteriorates, trigger a handover;
* Periodically handover all the active LVAPs in the network to the WTP which has the best link quality.  

The complete implementation of the Mobility Manager Network application discussed in this tutorial can be found [here](https://github.com/5g-empower/empower-runtime/tree/master/empower/apps/mobilitymanager/mobilitymanager.py).

<a name="prereq"/>

## Pre-requisites

In order to run this Network application the following requirements have to be met:

* The MAC-Adresses of your clients need to be whitelisted in the ACL-tab ([tutorial](https://github.com/5g-empower/5g-empower.github.io/wiki/Creating-your-first-slice-(virtual-network)#acl)).
* A Slice with at least two WTPs must be created ([tutorial](https://github.com/5g-empower/5g-empower.github.io/wiki/Creating-your-first-slice-(virtual-network))).
* The MobilityManager module has been loaded ([tutorial](https://github.com/5g-empower/5g-empower.github.io/wiki/Launching%20Apps))

<a name="init"/>

## Launch method 

The launch method for the Mobility Manager application is shown below:

``` python
def launch(tenant_id, every=DEFAULT_PERIOD):
    """ Initialize the module. """

    return MobilityManager(tenant_id=tenant_id, limit=limit, every=every)
```

As it can be seen the _launch_ method has two parameters: the mandatory _tenant_id_ parameter, and the _every_ parameter specifying the control task period.

<a name="handover"/>

## Handover

In this case the _wtp_up_ event is used in order to execute the _ucqm_ primitive when a WTP connects to the controller. This is needed in order to update the slice's network view.

The _wtpup_ trigger callbacks method is reported below:

```python
def wtp_up(self, wtp):
    for block in wtp.supports:
        self.ucqm(block=block, every=self.every)
```

The _loop_ method in the _MobilityManager_ class is called periodically with period defined by the _every_ parameter (in ms). The _loop_ method in turn calls the _handover_ method to handover every active LVAP within the network to the best WTP.  

```python
def loop(self):
    for lvap in self.lvaps():
        lvap.blocks = self.blocks().sort_by_rssi(lvap.addr).first()
```

The full mobility manager application can be seen [here](https://github.com/5g-empower/empower-runtime/blob/master/empower/apps/mobilitymanager/mobilitymanager.py).
