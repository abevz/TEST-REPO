# Configuring WireGuard on Edgerouter

You can see used resource in footnotes[^1]  


Install:

curl -OL https://github.com/WireGuard/wireguard-vyatta-ubnt/releases/download/1.0.20211208-1/e100-v2-v1.0.20211208-v1.0.20210914.deb

sudo dpkg -i e100-v2-v1.0.20211208-v1.0.20210914.deb

Usage:

wg genkey | tee /config/auth/wg.key | wg pubkey >  wg.public
R2XU0QS8PjKqujCTJgWGysrVXdmy5c0utnP0ziI2dm0=
```
configure

set interfaces wireguard wg0 address 192.168.33.1/24
set interfaces wireguard wg0 listen-port 51944
set interfaces wireguard wg0 route-allowed-ips true

set interfaces wireguard wg0 peer HdXPvZ7oiBx8iUx4YS2ldDCVGj7C+yl/YHt+xiT1ggE= endpoint albevz.synology.me:51944
set interfaces wireguard wg0 peer HdXPvZ7oiBx8iUx4YS2ldDCVGj7C+yl/YHt+xiT1ggE= allowed-ips 192.168.33.101/32
set interfaces wireguard wg0 peer HdXPvZ7oiBx8iUx4YS2ldDCVGj7C+yl/YHt+xiT1ggE= description "Android S20"

set interfaces wireguard wg0 peer 2QwtX5jr3DXXXhnlwFz8cv/as5n7D7/R371V2xnpmWY= endpoint albevz.synology.me:51944
set interfaces wireguard wg0 peer 2QwtX5jr3DXXXhnlwFz8cv/as5n7D7/R371V2xnpmWY= allowed-ips 192.168.33.102/32
set interfaces wireguard wg0 peer 2QwtX5jr3DXXXhnlwFz8cv/as5n7D7/R371V2xnpmWY= description "Android ST6"

set interfaces wireguard wg0 private-key /config/auth/wg.key

set firewall name WAN_LOCAL rule 50 action accept
set firewall name WAN_LOCAL rule 50 protocol udp
set firewall name WAN_LOCAL rule 50 description 'WireGuard'
set firewall name WAN_LOCAL rule 50 destination port 51944
#set firewall name WAN_LOCAL rule 20 log disable

commit
save
exit
```

Upgrade:

configure
set interfaces wireguard wg0 route-allowed-ips false
commit
delete interfaces wireguard
commit
sudo rmmod wireguard
sudo dpkg -i ${BOARD}-${RELEASE}.deb
sudo modprobe wireguard
load
commit
exit




```
#!/usr/bin/env bash
###############################################################################
#         File:  wg-update-peer.sh                                            #
###############################################################################
# Updates the Public IP address of my peer's Edgerouter running wireguard     #
#                                                                             #
# D.C 2021-02-07 - Updated the peer key                                       #
###############################################################################
#
sudo wg set wg0 peer FoVFgidfasdfasfsfassfdfsfssdeedddddddfffffffff= endpoint mypeer.somedns.org:51820
```

[^1]: Used resources.

 [Forum ubiquity](https://community.ui.com/questions/Release-WireGuard-for-EdgeRouter/3765d2a4-1952-4629-948a-3ac9d9c22311?page=48).

 [Git](https://github.com/WireGuard/wireguard-vyatta-ubnt).
