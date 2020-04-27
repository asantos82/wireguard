# Wireguard

This repository contains scripts that make it easy to configure [WireGuard](https://www.wireguard.com)
on [VPS](https://en.wikipedia.org/wiki/Virtual_private_server).

Medium article: [How to deploy WireGuard node on a DigitalOcean's droplet](https://medium.com/@drew2a/replace-your-vpn-provider-by-setting-up-wireguard-on-digitalocean-6954c9279b17)

## Quick Start

```bash
wget https://raw.githubusercontent.com/drew2a/wireguard/master/wg-ububtu-server-up.sh

chmod +x ./wg-ububtu-server-up.sh
./wg-ububtu-server-up.sh
```

To get a full instruction, please follow to the article above.

## wg-ubuntu-server-up.sh

This script:

* Installs all necessary software on an empty Ubuntu DigitalOcean droplet
(it should also work with most modern Ubuntu images)
* Configures IPv4 forwarding and iptables rules
* Sets up [unbound](https://github.com/NLnetLabs/unbound) DNS resolver 
* Creates a server and clients configurations
* Installs [qrencode](https://github.com/fukuchi/libqrencode/)
* Runs [WireGuard](https://www.wireguard.com)

### Usage

```bash
wg-ubuntu-server-up.sh [<number_of_clients>]
```

### Example of usage

```bash
./wg-ubuntu-server-up.sh
```

```bash
./wg-ubuntu-server-up.sh 10
```

## wg-genconf.sh

This script generate server and clients configs for WireGuard.

If the public IP is not defined, then the public IP of the machine from which 
the script is run is used.
If the number of clients is not defined, then used 10 clients.

### Prerequisites

Install [WireGuard](https://www.wireguard.com) if it's not installed.

### Usage

```bash
./wg-genconf.sh [<number_of_clients> [<server_public_ip>]]
```

### Example of usage:

```bash
./wg-genconf.sh
```

```bash
./wg-genconf.sh 10
```

```bash
./wg-genconf.sh 10 157.245.73.253 
```

## Contributors

* [buraksarica](https://github.com/buraksarica)

## Trick to make AllowedIPs work on OSX

On OSX if you want to excluded private IP rages wireguard just stops working.

- Tick the box "Exclude Private IPs"
- Grab the public of your instance running WireGuard i.e. 34.250.109.34
- On the Excluded IP Ranges find the range where your IPs belongs i.e. 32.0.0.0/3
- Open python and write the following for the examples above
```python
n1 = ipaddress.ip_network(u'32.0.0.0/3')
n2 = ipaddress.ip_network(u'34.250.109.34/32')
list(n1.address_exclude(n2))
[IPv4Network(u'48.0.0.0/4'), IPv4Network(u'40.0.0.0/5'), IPv4Network(u'36.0.0.0/6'), IPv4Network(u'32.0.0.0/7'), IPv4Network(u'35.0.0.0/8'), IPv4Network(u'34.0.0.0/9'), IPv4Network(u'34.128.0.0/10'), IPv4Network(u'34.192.0.0/11'), IPv4Network(u'34.224.0.0/12'), IPv4Network(u'34.240.0.0/13'), IPv4Network(u'34.252.0.0/14'), IPv4Network(u'34.248.0.0/15'), IPv4Network(u'34.251.0.0/16'), IPv4Network(u'34.250.128.0/17'), IPv4Network(u'34.250.0.0/18'), IPv4Network(u'34.250.64.0/19'), IPv4Network(u'34.250.112.0/20'), IPv4Network(u'34.250.96.0/21'), IPv4Network(u'34.250.104.0/22'), IPv4Network(u'34.250.110.0/23'), IPv4Network(u'34.250.108.0/24'), IPv4Network(u'34.250.109.128/25'), IPv4Network(u'34.250.109.64/26'), IPv4Network(u'34.250.109.0/27'), IPv4Network(u'34.250.109.48/28'), IPv4Network(u'34.250.109.40/29'), IPv4Network(u'34.250.109.36/30'), IPv4Network(u'34.250.109.32/31'), IPv4Network(u'34.250.109.35/32')]
```

- Go back to the Excluped IPs Ranges previosuly generated and replace i.e 32.0.0.0/3 with all the IPs Ranges from above, i.e. 48.0.0.0/4, 40.0.0.0/5, 36.0.0.0/6 ... and so on

