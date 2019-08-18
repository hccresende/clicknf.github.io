# Table of Contents
1. [Hardware Requirements](#requiredhardware)
2. [Software Requirements](#requiredpackages)
3. [Compilation process](#compile)
3. [Configuration process](#configure)
3. [Start the VBS](#run)

<a name="requiredhardware"/>

## Hardware Requirements

This guide assumes that you are deploying the eNodeB using our pre-patched version of the srsLTE software stack and that you have access to a standard LTE EPC, e.g. EURECOM OpenAirInterface EPC implementation.

For the machine running the eNB a Quad core PC (i5 or better) with at least 8 GM RAM will be needed together with a Software Defined Radio platform supported by srsLTE (e.g. the Ettus USRP B210).

The reference operating system for this guide is Ubuntu 18.04.

<a name="requiredpackages"/>

## Software Requirements

The following commands will update the package repository and install some dependencies:

```shell
sudo apt-get update
sudo apt-get install build-essential libpthread-stubs0-dev cmake \
                     libfftw3-dev libmbedtls-dev libboost-all-dev \
                     libconfig++-dev libsctp-dev libuhd-dev
```
<a name="compile"/>

## Compilation process

Download, compile, and install the EmPOWER protocol definition:

```
cd ~
git clone https://github.com/5g-empower/empower-enb-proto.git
cd empower-enb-proto
make
sudo make install
```

Download, compile, and install the EmPOWER protocol definition:

```
cd ~
git clone https://github.com/5g-empower/empower-enb-agent.git
cd empower-enb-agent
make
sudo make install
```

Download and compile the srsLTE code:

```
cd ~
git clone https://github.com/5g-empower/empower-srsLTE.git
cd empower-srsLTE
mkdir build 
cd build 
cmake ../ 
make
```

<a name="configure"/>

## Configuration process

This tutorial assumes that you have an compatible EPC at your disposal. This could be either a commercial EPC or an open-source one. Please refer to your EPC provider for information about its configuration and usage.

Copy the example configuration files into the working directory:

```
cd ~/empower-srsLTE
cp srsenb/drb.conf.example build/srsenb/src/drb.conf
cp srsenb/enb.conf.example build/srsenb/src/enb.conf
cp srsenb/rr.conf.example build/srsenb/src/rr.conf
cp srsenb/sib.conf.example build/srsenb/src/sib.conf
```

Leave all the configuration files unchanged with the exception of the `enb.conf` file which has to be edited in order to specify:

* the desired enb_id and cell_id
* the phy_cell_id compatible with the cell_id
* the TAC, MCC, and MNC as defined in the core network configuration
* the mme_addr pointing to the MME in the core network
* the gtp_bind_addr pointing to the local IP address used to reach the core network
* the number of resource blocks assigned to the cell (n_prb)
* the controller address (ctrl_addr) and port (ctrl_port, default 2210)

<a name="run "/>

## Start the VBS

Start the srsenb:

```
cd ~/empower-srsLTE/build/srsenb/src/
./srsenb enb.conf
```