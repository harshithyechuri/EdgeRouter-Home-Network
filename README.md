# EdgeRouter-Home-Network
EdgeRouter Config Commands For A Perfect Home Network

***Switch to Config mode***
```
configure
```


***Creating VLAN's for the Network***
```
set interfaces ethernet eth0 description 'Internet (ISP)'
set interfaces ethernet eth1 description 'Lan Network'
set interfaces ethernet eth1 vif 10 address 10.10.10.1/24
set interfaces ethernet eth1 vif 10 description 'Main Network'
set interfaces ethernet eth1 vif 20 address 10.10.20.1/24
set interfaces ethernet eth1 vif 20 description 'Guest Network'
set interfaces ethernet eth1 vif 30 address 10.10.30.1/24
set interfaces ethernet eth1 vif 30 description 'IoT Network''
commit
```

***Creating DHCP Server for VLAN's***
```
set service dhcp-server disabled false
set service dhcp-server shared-network-name Main-V10
set service dhcp-server shared-network-name Main-V10 authoritative enable
set service dhcp-server shared-network-name Main-V10 subnet 10.10.10.0/24 default-router 10.10.10.1
set service dhcp-server shared-network-name Main-V10 subnet 10.10.10.0/24 dns-server 8.8.8.8
set service dhcp-server shared-network-name Main-V10 subnet 10.10.10.0/24 lease 86400
set service dhcp-server shared-network-name Main-V10 subnet 10.10.10.0/24 start 10.10.10.11 stop 10.10.10.210

set service dhcp-server shared-network-name Guest-V20
set service dhcp-server shared-network-name Guest-V20 authoritative enable
set service dhcp-server shared-network-name Guest-V20 subnet 10.10.20.0/24 default-router 10.10.20.1
set service dhcp-server shared-network-name Guest-V20 subnet 10.10.20.0/24 dns-server 8.8.8.8
set service dhcp-server shared-network-name Guest-V20 subnet 10.10.20.0/24 lease 86400
set service dhcp-server shared-network-name Guest-V20 subnet 10.10.20.0/24 start 10.10.20.11 stop 10.10.20.210

set service dhcp-server shared-network-name IoT-V30
set service dhcp-server shared-network-name IoT-V30 authoritative enable
set service dhcp-server shared-network-name IoT-V30 subnet 10.10.30.0/24 default-router 10.10.30.1
set service dhcp-server shared-network-name IoT-V30 subnet 10.10.30.0/24 dns-server 8.8.8.8
set service dhcp-server shared-network-name IoT-V30 subnet 10.10.30.0/24 lease 86400
set service dhcp-server shared-network-name IoT-V30 subnet 10.10.30.0/24 start 10.10.30.11 stop 10.10.30.210

commit
```

***Setting up FIREWALL for Guest & IOT VLAN's***

*Setting up "Protected Networs"*
```
set firewall group network-group LAN_NETWORKS description 'Private Network Group'
set firewall group network-group LAN_NETWORKS network 192.168.0.0/16
set firewall group network-group LAN_NETWORKS network 172.16.0.0/12
set firewall group network-group LAN_NETWORKS network 10.0.0.0/8
commit
```
*Setting up rules for GUEST VLAN*
```
set firewall name GUEST_IN
set firewall name GUEST_IN description 'Guest to Internet' 
set firewall name GUEST_IN default-action accept
set firewall name GUEST_IN rule 10 action accept
set firewall name GUEST_IN rule 10 description 'Allow'
set firewall name GUEST_IN rule 10 protocol all
set firewall name GUEST_IN rule 10 state established enable
set firewall name GUEST_IN rule 10 state related enable
set firewall name GUEST_IN rule 20 action drop
set firewall name GUEST_IN rule 20 description 'Drop'
set firewall name GUEST_IN rule 20 destination group network-group LAN_NETWORKS
set firewall name GUEST_IN rule 20 protocol all

set firewall name GUEST_LOCAL
set firewall name GUEST_LOCAL description 'Guest to Internet'
set firewall name GUEST_LOCAL default-action drop
set firewall name GUEST_LOCAL rule 10 action accept
set firewall name GUEST_LOCAL rule 10 description 'Allow DNS'
set firewall name GUEST_LOCAL rule 10 log disable
set firewall name GUEST_LOCAL rule 10 protocol tcp_udp
set firewall name GUEST_LOCAL rule 10 destination port 53
set firewall name GUEST_LOCAL rule 20 action accept
set firewall name GUEST_LOCAL rule 20 description 'Allow DHCP'
set firewall name GUEST_LOCAL rule 20 log disable
set firewall name GUEST_LOCAL rule 20 protocol udp
set firewall name GUEST_LOCAL rule 20 destination port 67
commit
```
*Setting up rules for IOT VLAN*
```
set firewall name IoT_IN
set firewall name IoT_IN description 'IoT to Internet' 
set firewall name IoT_IN default-action accept
set firewall name IoT_IN rule 10 action accept
set firewall name IoT_IN rule 10 description 'Allow'
set firewall name IoT_IN rule 10 protocol all
set firewall name IoT_IN rule 10 state established enable
set firewall name IoT_IN rule 10 state related enable
set firewall name IoT_IN rule 20 action drop
set firewall name IoT_IN rule 20 description 'Drop'
set firewall name IoT_IN rule 20 destination group network-group LAN_NETWORKS
set firewall name IoT_IN rule 20 protocol all

set firewall name IoT_LOCAL
set firewall name IoT_LOCAL description 'IoT to Internet'
set firewall name IoT_LOCAL default-action drop
set firewall name IoT_LOCAL rule 10 action accept
set firewall name IoT_LOCAL rule 10 description 'Allow DNS'
set firewall name IoT_LOCAL rule 10 log disable
set firewall name IoT_LOCAL rule 10 protocol tcp_udp
set firewall name IoT_LOCAL rule 10 destination port 53
set firewall name IoT_LOCAL rule 20 action accept
set firewall name IoT_LOCAL rule 20 description 'Allow DHCP'
set firewall name IoT_LOCAL rule 20 log disable
set firewall name IoT_LOCAL rule 20 protocol udp
set firewall name IoT_LOCAL rule 20 destination port 67
commit
```
*Assinging the VLAN interfaces*
```
set interfaces ethernet eth1 vif 20 firewall in name GUEST_IN
set interfaces ethernet eth1 vif 20 firewall local name GUEST_LOCAL

set interfaces ethernet eth1 vif 30 firewall in name IoT_IN
set interfaces ethernet eth1 vif 30 firewall local name IoT_LOCAL

commit
save
exit
```
