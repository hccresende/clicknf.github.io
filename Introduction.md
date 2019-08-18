This section documents how to setup a basic testbed. In particular:
* [Network Setup](Network-setup), will explain which are the basic network setups that can be used.
* [Setting up the WTP](Setting-up-the-WTP), will explain how to build a WTP (which is a Wi-Fi access point controlled by the empower-runtime).
* [Setting up the CPP](Setting-up-the-CPP), will explain how to configure a generic Linux machine to act as Click Packet Processor (CPP). Notice that this part is needed only if you want to run Light Virtual Network Functions (LVNFs) in your network.
* [Setting up the VBS](Setting-up-the-VBS), will explain how to build a VBS (which is an LTE eNodeB controlled by the empower-runtime).
* [Setting up the 5G-EmPOWER Controller](Setting-up-the-EmPOWER-Runtime), will explain how to setup the 5G-EmPOWER Controller.
* [Setting up the Backhaul Controller](Setting-up-the-Backhaul-Controller), will explain how to setup a backhaul controller. Notice that this part is needed only if you want to run Light Virtual Network Functions (LVNFs) in your network.
* [Setting up a Network Slice](Setting-up-a-Network-Slice), will explain how to create your first network slice.

For more information about the SDK primitives to manage a Wi-Fi network you can check [here](Python-Wi-Fi-API-documentation), if you want more information about the SDK primitives to manage LVNFs you can check [here](Python-Click-API-documentation), finally if you want more information about the SDK primitives to manage LTE networks you can check [here](Python-LTE-API-documentation).

The only type of backhaul network supported for the moment is an OpenFlow controlled network and the only OpenFLow controller supported at this time is Ryu.

The 5G-EmPOWER controller communicates with Ryu using an intent based networking interface included in the version of Ryu that can be downloaded from this repository. No additional configuration is needed if both the Ryu controller and the 5G-EmPOWER controller are executed on the same machine. More information about the 5G-EmPOWER intent based networking interface can be found [here](EmPOWER-IBN).
