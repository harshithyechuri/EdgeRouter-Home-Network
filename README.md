# EdgeRouter-Home
EdgeRouter Config Commands For A Perfect Home Network

***Creating VLAN's for the Network***
```
configure
set interfaces ethernet eth0 description 'Internet (ISP)'
set interfaces ethernet eth1 description 'Lan Network'
set interfaces ethernet eth1 vif 10 address 10.10.10.1/24
set interfaces ethernet eth1 vif 10 description 'Main Network'
set interfaces ethernet eth1 vif 20 address 10.10.20.1/24
set interfaces ethernet eth1 vif 20 description 'Guest Network'
set interfaces ethernet eth1 vif 30 address 10.10.30.1/24
set interfaces ethernet eth1 vif 30 description 'IoT Network''
commit
save
```
