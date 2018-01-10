#virtualNetwork

**UBUNTU 16.04:** Ubuntu_16.04.3-VM-64bit.vmdk

**ROUTER CISCO 3725:** c3725-adventerprisek9-mz.124-15.T14.image

**ROUTER R2**
```

conf t 
int f0/0
ip address 10.10.10.4 255.255.255.0
no shut
end
wr
conf t
int f0/1
ip address dhcp
ip nat outside
no shut
int f0/0
ip nat inside
ip nat inside source list 1 int f0/1 overload
access-list 1 permit 10.10.10.0 0.0.0.255
end
wr
conf t
router ospf 1
network 10.10.10.0 0.0.0.255 area 0
end
wr
```

**ROUTER R1**
```

conf t
int f0/0
ip address 10.10.10.5 255.255.255.0
no shut
end
wr
conf t
ip dhcp pool DHCP-R1
network 192.168.1.0 /24
dns-server 192.168.1.1
default-router 192.168.1.1
lease 8
ip dhcp excluded-address 192.168.1.11
int f0/1
ip address 192.168.1.1 255.255.255.0
end
wr
conf t
ip domain-lookup
ip route 0.0.0.0 0.0.0.0 10.10.10.4
ip name-server 8.8.8.8
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
network 10.10.10.0 0.0.0.255 area 0
default-information originate
end
wr

```

**UBUNTU-2**
```

sudo gedit /etc/resolv.conf
(nameserver 8.8.8.8)

sudo apt-get update
sudo apt-get install apache2
sudo systemctl restart apache2

```
**UBUNTU-3**
```

sudo gedit /etc/resolv.conf
(nameserver 8.8.8.8)

sudo apt-get update
sudo apt-get install tcpdump

```
