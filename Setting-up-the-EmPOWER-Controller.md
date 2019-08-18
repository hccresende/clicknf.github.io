# Table of Contents
1. [Hardware Requirements](#requiredhardware)
2. [Software Requirements](#requiredpackages)
3. [Running Controller](#runningcontroller)
4. [Configuration Process](#configuration)

<a name="requiredhardware"/>

## Hardware Requirements

One PC running a recent Linux distribution. There are no particular hardware requirements. Any reasonably recent laptop/desktop should be able to run the controller.

The reference operating system for this guide is Ubuntu 18.04.

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
sudo pip3 install tornado sqlalchemy construct==2.5.5-reupload
```

<a name="runningcontroller"/>

## Running the Controller

Checkout the EmPOWER Controller repository:

```
git clone https://github.com/5g-empower/empower-runtime.git
```

The EmPOWER controller must be executed from a central server that can be reached from all WTPs, CPPs, and VBSes. From the main repository enter the controller directory:

```
cd empower-runtime
```

create a directory for the database named deploy:

```
mkdir deploy
```

and start the controller:

```
./empower-runtime.py
```

If all the python dependencies are satisfied you should see an output like this:

``` shell
INFO:core:Starting EmPOWER Runtime
INFO:core:Generating default accounts
INFO:core:Loading EmPOWER Runtime defaults
INFO:root:Importing module: empower.restserver.restserver
INFO:root:Importing module: empower.lvnfp.lvnfpserver
INFO:root:Importing module: empower.lvapp.lvappserver
INFO:root:Importing module: empower.vbsp.vbspserver
INFO:root:Importing module: empower.lvapp.lvap_stats.lvap_stats
INFO:root:Importing module: empower.lvapp.bin_counter.bin_counter
INFO:root:Importing module: empower.lvapp.wtp_bin_counter.wtp_bin_counter
INFO:root:Importing module: empower.lvapp.txp_bin_counter.txp_bin_counter
INFO:root:Importing module: empower.lvapp.trq_bin_counter.trq_bin_counter
INFO:root:Importing module: empower.lvapp.cqm.ucqm
INFO:root:Importing module: empower.lvapp.cqm.ncqm
INFO:root:Importing module: empower.lvapp.wifi_stats.wifi_stats
INFO:root:Importing module: empower.lvapp.rssi.rssi
INFO:root:Importing module: empower.lvapp.summary.summary
INFO:root:Importing module: empower.vbsp.rrc_measurements.rrc_measurements
INFO:root:Importing module: empower.vbsp.mac_reports.mac_reports
INFO:root:Importing module: empower.lvnfp.lvnf_get.lvnf_get
INFO:root:Importing module: empower.lvnfp.lvnf_set.lvnf_set
INFO:root:Importing module: empower.lvnfp.lvnf_stats.lvnf_stats
INFO:core:Registering 'empower.restserver.restserver'
INFO:core.service:REST Server available at 8888
INFO:core:Registering 'empower.lvnfp.lvnfpserver'
INFO:core.service:LVNF Server available at 4422
INFO:core:Registering 'empower.lvapp.lvappserver'
INFO:core.service:LVAP Server available at 4433
INFO:core:Registering 'empower.vbsp.vbspserver'
INFO:core.service:VBSP Server available at 2210
INFO:core:Registering 'empower.ibnp.ibnpserver'
INFO:core:Registering 'empower.lvapp.lvap_stats.lvap_stats'
INFO:core:Registering 'empower.lvapp.bin_counter.bin_counter'
INFO:core:Registering 'empower.lvapp.wtp_bin_counter.wtp_bin_counter'
INFO:core:Registering 'empower.lvapp.txp_bin_counter.txp_bin_counter'
INFO:core:Registering 'empower.lvapp.trq_bin_counter.trq_bin_counter'
INFO:core:Registering 'empower.lvapp.cqm.ucqm'
INFO:core:Registering 'empower.lvapp.cqm.ncqm'
INFO:core:Registering 'empower.lvapp.wifi_stats.wifi_stats'
INFO:core:Registering 'empower.lvapp.rssi.rssi'
INFO:core:Registering 'empower.lvapp.summary.summary'
INFO:core:Registering 'empower.vbsp.rrc_measurements.rrc_measurements'
INFO:core:Registering 'empower.vbsp.mac_reports.mac_reports'
INFO:core:Registering 'empower.lvnfp.lvnf_get.lvnf_get'
INFO:core:Registering 'empower.lvnfp.lvnf_set.lvnf_set'
INFO:core:Registering 'empower.lvnfp.lvnf_stats.lvnf_stats'
```

If some Python package is missing you should install it using either the Python package managed (pip) or your distribution package manager. Make sure to install the Python3 version of the dependencies.

The controller web interface can be reached at: http://127.0.0.1:8888/

![Adding a WTP Main Menu](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/ControllerMainPage.png)

<a name="configuration"/>

## Configuration Process

The first time the controller is initialized, three accounts are automatically created:

| Username | Password | Type          |
|----------|----------|---------------|
| root     | root     | Administrator |
| foo      | foo      | User          |
| bar      | bar      | User          |

The first account has administrator privileges, while the other two are user accounts.

### Adding the WTPs/CPPs/VBSes

Log in as root and click on the Device menu entry and then on WTPs, CPPs, or VBSes.

![Adding a WTP Main Menu](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/ControllerWTPsMainPage.png)

Click on the (+) icon and specify the MAC address of the device that is authorised to connect to this controller. Notice how you can also add a human readable description of the device, e.g. "First Floor, Meeting Room"

![Adding a WTP](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/ControllerAddNewWTP.png)

After all devices have been added and if the controller is reachable by the devices on the IP address specified in their configuration files then you should see an output like this in the controller terminal:

```
INFO:core.service:Incoming connection from ('192.168.1.5', 42550)
INFO:lvapp.lvappconnection:Got hello message from 00:0D:B9:2F:56:64 seq 9335
INFO:core.pnfdev:PNFDev 00:0D:B9:2F:56:64 mode disconnected->connected
INFO:lvapp.lvappconnection:Sending caps_request message to 00:0D:B9:2F:56:64 at 192.168.1.5 last_seen 0 seq 1
INFO:lvapp.lvappconnection:Got caps message from 00:0D:B9:2F:56:64 seq 9336
INFO:core.pnfdev:PNFDev 00:0D:B9:2F:56:64 mode connected->online
```

The same procedure applies also for adding CPPs and/or VBSes.

The device identifiers are the following

- WTPs: the MAC address of the OpenVSwitch bridge (br-ovs).
- CPPs: the MAC address of the OpenVSwitch bridge (br0).
- VBSes: the eNodeB id represented as a MAC address, for example if the eNodeB id is 0xe21 then its MAC address representation will be 00:00:00:00:0E:21. 

### Access control

The open source version of EmPOWER does not support WPA authentication, so you must specify the list of MAC address that can connect to any of slices defined in the controller (by default nobody can associate). In order to do so click on the "ACL" tab and specify the clients MAC addresses that are allowed to use the network.

![Access control](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/ControllerAddACL.png)

### Creating a virtual network

The EmPOWER Controller can run multiple virtual networks on top of the same physical infrastructure. 

Virtual Networks can be created and deleted only by administrators.

Log in as root and click on the Admin menu entry and then on Tenants.

![List of virtual network](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/ControllerTenantsMainPage.png)

Click on the (+) icon and specify a name (this will be your WLAN SSID), an optional short description, and select the owner of this network from the drop down list. Leave the BSSID set to "Unique". Then click on the button "Save". 

![Add Tenant](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/ControllerAddNewTenant.png)

Notice how from this form you can also specify a PLMN id. This is necessary if you plan to use LTE features of the platform. Otherwise this field can be left empty. If specified the PLMN id must be the same of the one specified in the EPC configuration.

At this point any of the clients that are allowed to use the network should see the new SSID and should be able to associate to it and to reach the public Internet (if the wireless APs have a backhaul that is connected to the Internet).