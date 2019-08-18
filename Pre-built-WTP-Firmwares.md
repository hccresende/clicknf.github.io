We release a set of firmwares for a selected range of platforms. They are basically the routers that have been tested in our lab. The default root password for all the images is `empower`. All images are pre-configured to use `192.168.100.158` as controller.

The following platforms are supported:

1. [PC Engines Alix 2D](#alix) ([website](http://www.pcengines.ch/alix.htm))
2. [PC Engines APU 2C](#apu) ([website](http://www.pcengines.ch/apu2.htm)) 
3. [TP-Link WDR4300](#wdr4300) ([website](http://www.tp-link.it/products/details/TL-WDR4300.html))
4. [D-Link DIR 505A](#dir505a) ([website](http://www.dlink.com/uk/en/products/dir-505-shareport-mobile-companion))
5. [Netgear WNDR 4300](#wndr4300) ([website](http://www.netgear.it/home/products/networking/wifi-routers/wndr4300.aspx))

We will add more pre-built firmware in the near future. Please contact us if you would like to see a particular platform officially supported. Please consider that we will upload only firmwares that we can test, meaning that we must have the actual wireless router in our lab. 

Firmwares can be downloaded from the [releases](https://github.com/5g-empower/empower-openwrt-15.05/releases) section. Instructions for uploading them onto the wireless routers are provided in the following sections.

<a name="alix"/>

## PC Engines Alix 2D

Alix boards are manufactured and sold by PC Engines. They utilize the x86-based AMD Geode CPUs. The boards and other components (enclosures, power supplies, wireless cards, CompactFlash cards, etc) are sold separately, although some retailers sell pre-build kits.

A picture of one of the Alix WTPs in our lab is reported below. Notice how they are equipped with two Wireless 802.11n NICs, hence the reason for the four antennas. 

![PC Engines ALIX 2D WTP](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/wtp_alix_2d.jpg)

The EmPOWER firmware downloaded from the releases section:

* [openwrt-x86-geode-combined-squashfs.img](https://github.com/5g-empower/empower-openwrt-15.05/releases/download/v20170803/openwrt-x86-geode-combined-squashfs.img)

The easiest method of installation for ALIX boards is to use the ``dd`` command. Notice that after you do this the first time you can also update the firmware using the router web interface. Attach a USB CompactFlash reader/writer to the flashing machine and insert the CompactFlash card.

Watch the output from the ``dmesg`` command to see what device ID it has picked up (it will be something like /dev/sdb). Ensure that /dev/sdb (or whatever is determined in the previous step) is not mounted through the use of the 'mount' command. If it is, unmount it with ``umount``.

Flash the image to the CompactFlash card with:

``` shell 
dd if=openwrt-x86-geode-combined-squashfs.img of=/dev/sdb
```

Removed the CompactFlash card, place it in the ALIX board and power on.

The eth0 interface (the one close to the power plug) is the WAN interface and it is configured to obtain an IP address from a DHCP server. 

The eth1 interface is the LAN interface and is configured to give IP addresses on the 192.168.1.x subnet. The wireless router itself is reachable from the LAN interface at the 192.168.1.1. 

<a name="apu"/>

## PC Engines APU 2C

APU boards are manufactured and sold by PC Engines. Boards are based on AMD Bobcat low consumption CPU. The boards and other components (enclosures, power supplies, wireless cards, CompactFlash cards, etc) are sold separately, although some retailers sell pre-build kits.

A picture of one of the APU WTPs in our lab is reported below. Notice how they are equipped with two Wireless 802.11n NICs, hence the reason for the four antennas. 

![PC Engines APU WTP](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/wtp_apu.jpg)

The EmPOWER firmware downloaded from the releases section:

* [openwrt-x86-64-combined-squashfs.img](https://github.com/5g-empower/empower-openwrt-15.05/releases/download/v20170803/openwrt-x86-64-combined-squashfs.img)

The easiest method of installation for ALIX boards is to use the ``dd`` command. Notice that after you do this the first time you can also update the firmware using the router web interface. Please refer to the flashing procedure for ALIX boards for the detailed instructions. 

Notice that in the APU the ethernet interface order is reversed and that the eth0 interface is the one next to the serial interface.

<a name="wdr4300"/>

## TP-Link WDR 4300

The TP-Link WDR4300 is a commercial WiFi router equipped with two 802.11n interfaces and Gigabit Ethernet. The router supports Dual-Stream (2x2) on the 2.4 Ghz Band and Triple-Stream (3x3) on the 5 Ghz Band. 

A picture of one of the WDR4300 WTPs in our lab is reported below. 

![TP-Link WDR4300 WTP](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/wtp_wdr4300.jpg)

The EmPOWER firmware can be uploaded directly from the router web interface. Notice that two different firmwares are available in our releases section:

* [openwrt-ar71xx-generic-tl-wdr4300-v1-squashfs-factory.bin](https://github.com/5g-empower/empower-openwrt-15.05/releases/download/v20170803/openwrt-ar71xx-generic-tl-wdr4300-v1-squashfs-factory.bin), this must be used only for upgrading from the stock firmware to the empower firmware.
* [openwrt-ar71xx-generic-tl-wdr4300-v1-squashfs-sysupgrade.bin](https://github.com/5g-empower/empower-openwrt-15.05/releases/download/v20170803/openwrt-ar71xx-generic-tl-wdr4300-v1-squashfs-sysupgrade.bin), this can be used to re-flash a router that is already using the EmPOWER firmware, for example because you was to reset to the initial configuration or because a new version of the firmware has been release.

Download the firmware from using the suitable link above and rename it to the format that the original firmware expects. Something like: wdr4300v1_en_3_14_3_up_boot(150518).bin. Otherwise the page will show error messages like "please select a file to upgrade".

Connect your PC to a LAN port of the TP-link via ethernet.

Login to the TP-link web administration webpage. (Default address is 192.168.0.1)

Under 'System Tools' select 'Firmware Upgrade'. Browse to the previously downloaded *.bin file. Click Upgrade.

<a name="dir505a"/>

## D-Link DIR 505A

The D-Link DID505A is a commercial WiFi router equipped with a single 802.11g (2.4GHz only). This wireless router is very cheap an can usually be found for less than 20 Euros.

A picture of two of the DID505A WTPs in our lab is reported below. 

![D-Link DIR 505A](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/wtp_dir505a.jpg)

The EmPOWER firmware can be uploaded directly from the router web interface. Notice that two different firmwares are available in our releases section:

* [openwrt-ar71xx-generic-dir-505-a1-squashfs-factory.bin](https://github.com/5g-empower/empower-openwrt-15.05/releases/download/v20170803/openwrt-ar71xx-generic-dir-505-a1-squashfs-factory.bin), this must be used only for upgrading from the stock firmware to the empower firmware.
* [openwrt-ar71xx-generic-dir-505-a1-squashfs-sysupgrade.bin](https://github.com/5g-empower/empower-openwrt-15.05/releases/download/v20170803/openwrt-ar71xx-generic-dir-505-a1-squashfs-sysupgrade.bin), this can be used to re-flash a router that is already using the EmPOWER firmware, for example because you was to reset to the initial configuration or because a new version of the firmware has been release.

The procedure to upload the EmPOWER firmware is the same of the WDR 4300 one. In this case however there is no need to rename the file.

Notice also that this wireless router has a single Ethernet interface which is configured as WAN.

<a name="wndr4300"/>

## Netgear WNDR 4300

The Netgear WNDR 4300 is a commercial WiFi router equipped with two 802.11n interfaces and Gigabit Ethernet. The router supports Dual-Stream (2x2) on the 2.4 Ghz Band and Triple-Stream (3x3) on the 5 Ghz Band. 

A picture of one of the WNDR 4300 WTPs in our lab is reported below. 

![Netgear WNDR 4300](https://raw.githubusercontent.com/wiki/5g-empower/5g-empower.github.io/figures/wtp_wndr4300.jpg)

The EmPOWER firmware can be uploaded directly from the router web interface. Notice that two different firmwares are available in our releases section:

* [openwrt-ar71xx-nand-wndr4300-ubi-factory.img](https://github.com/5g-empower/empower-openwrt-15.05/releases/download/v20170803/openwrt-ar71xx-nand-wndr4300-ubi-factory.img), this must be used only for upgrading from the stock firmware to the empower firmware.
* [openwrt-ar71xx-nand-wndr4300-squashfs-sysupgrade.tar](https://github.com/5g-empower/empower-openwrt-15.05/releases/download/v20170803/openwrt-ar71xx-nand-wndr4300-squashfs-sysupgrade.tar), this can be used to re-flash a router that is already using the EmPOWER firmware, for example because you was to reset to the initial configuration or because a new version of the firmware has been release.

The procedure to upload the EmPOWER firmware is the same of the WDR 4300 one. In this case however there is no need to rename the file.
