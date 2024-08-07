## Features

- Create an AP (Access Point) at any channel.
- Choose one of the following encryptions: WPA, WPA2, WPA/WPA2, Open (no encryption).
- Hide your SSID.
- Disable communication between clients (client isolation).
- IEEE 802.11n & 802.11ac support
- Internet sharing methods: NATed or Bridged or None (no Internet sharing).
- Choose the AP Gateway IP (only for 'NATed' and 'None' Internet sharing methods).
- You can create an AP with the same interface you are getting your Internet connection.
- You can pass your SSID and password through pipe or through arguments (see examples).
- Optionally assign IP addresses to specified hosts via dnsmasq's `dhcp-host` configuration option.

## Dependencies

### General

- bash (to run this script)
- util-linux (for getopt)
- procps or procps-ng
- hostapd
- iproute2
- iw
- iwconfig (you only need this if 'iw' can not recognize your adapter)
- haveged (optional)

### For 'NATed' or 'None' Internet sharing method

- dnsmasq
- iptables

## Installation

### Generic

    git clone https://github.com/lakinduakash/linux-wifi-hotspot
    cd linux-wifi-hotspot/src/scripts
    make install

### ArchLinux

    pacman -S create_ap

### Gentoo

    emerge layman
    layman -f -a jorgicio
    emerge net-wireless/create_ap

## Examples

### No passphrase (open network):

    create_ap wlan0 eth0 MyAccessPoint

### WPA + WPA2 passphrase:

    create_ap wlan0 eth0 MyAccessPoint MyPassPhrase

### AP without Internet sharing:

    create_ap -n wlan0 MyAccessPoint MyPassPhrase

### Bridged Internet sharing:

    create_ap -m bridge wlan0 eth0 MyAccessPoint MyPassPhrase

### Bridged Internet sharing (pre-configured bridge interface):

    create_ap -m bridge wlan0 br0 MyAccessPoint MyPassPhrase

### Internet sharing from the same WiFi interface:

    create_ap wlan0 wlan0 MyAccessPoint MyPassPhrase

### Choose a different WiFi adapter driver

    create_ap --driver rtl871xdrv wlan0 eth0 MyAccessPoint MyPassPhrase

### No passphrase (open network) using pipe:

    echo -e "MyAccessPoint" | create_ap wlan0 eth0

### WPA + WPA2 passphrase using pipe:

    echo -e "MyAccessPoint\nMyPassPhrase" | create_ap wlan0 eth0

### Enable IEEE 802.11n

    create_ap --ieee80211n --ht_capab '[HT40+]' wlan0 eth0 MyAccessPoint MyPassPhrase

### Client Isolation:

    create_ap --isolate-clients wlan0 eth0 MyAccessPoint MyPassPhrase

### Assign IP address to a hosts listed in /etc/hosts:

    create_ap --dhcp-hosts "host1 host2" wlan0 eth0 MyAccessPoint MyPassPhrase

### Assign IP address to two hosts not listed in /etc/hosts:

    create_ap --dhcp-hosts "host1,192.168.12.2 host2,192.168.12.3" wlan0 eth0 MyAccessPoint MyPassPhrase

Use a space to separate multiple `dhcp-host` definitions. Reference [dnsmasq.conf.example](https://github.com/imp/dnsmasq/blob/770bce967cfc9967273d0acfb3ea018fb7b17522/dnsmasq.conf.example#L238) for other valid ways to define hosts, such as by MAC address.

## Systemd service

Using the persistent [systemd](https://wiki.archlinux.org/index.php/systemd#Basic_systemctl_usage) service

### Start service immediately:

    systemctl start create_ap

### Start on boot:

    systemctl enable create_ap

## License

FreeBSD

- Copyright (c) 2013, oblique
- Copyright (c) 2019, lakinduakash
