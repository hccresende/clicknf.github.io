# Welcome to the 5G-EmPOWER wiki!

**Please use the sidebar on the right to navigate through this WIKI.**

5G-EmPOWER is an open Mobile Network Operating System. Its flexible architecture and the high-level programming APIs allow for fast prototyping of novel services and applications.

The 5G-EmPOWER Mobile Network Operating System consists of the following components:

* empower-runtime, the Python-based 5G-EmPOWER controller. This allows programming Wi-Fi AP, LTE eNBs, and CPPs by writing network apps using either the [REST](https://github.com/5g-empower/5g-empower.github.io/wiki/REST-API-documentation) interface or the [Python](https://github.com/5g-empower/5g-empower.github.io/wiki/Python-API-documentation) SDK.
* empower-lvap-agent, the 5G-EmPOWER Wi-Fi agent. This agent allows controlling Wi-Fi access points using the empower-runtime. While in principle it is possible to install this agent on any Linux box you will avoid a lot of pain if you use our LEDE branch which includes all the necessary Kernel patches.
* empower-lvnf-agent, the 5G-EmPOWER LVNF agent. This agent allows deploying and managing small Virtual Network Function written using Click. The agent is written in Python and can run on any Linux box. Click must be pre-installed.
* empower-enb-agent, the 5G-EmPOWER LTE agent library. This agent allows controlling LTE eNBs using the empower-runtime. The agent can be integrated with any LTE stack but as of now, it is available only for srsLTE.
* empower-enb-proto, the OpenEmpower LTE protocol library. The protocols used by the empower-enb-agent.
* empower-srsLTE, a branch of srsLTE with the 5G-EmPOWER eNB agent.
* empower-lede-packages, the 5G-EmPOWER LVAP agent package for LEDE 17.01.
* empower-lede, a branch of LEDE 17.01 including some patches and the empower-lede-packages feed.
* empower-ryu, a branch of the Ryu controller with the 5G-EmPOWER [Intent-based Networking](https://github.com/5g-empower/5g-empower.github.io/wiki/EmPOWER-IBN) Interface.
* empower-manager, the 5G-EmPOWER Command Line Interface.
* empower-config, the configuration files for the Wi-Fi WTPs.
* empower-ctrl-discover, a command line utility for retrieving the control advertisement frames
* empower-simple-xmlrpc, a demo XML-RPC application using the EmPOWER REST API.

5G-EmPOWER is being developed by the Wireless and Networked Systems ([WiN](http://create-net.fbk.eu/win)) research unit at [FBK](http://www.fbk.eu/).