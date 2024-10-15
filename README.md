These configuration files are a base setup to create a travel wireless router that supports VLANS over a trunk LAN port that can then trunk to a vlan-enabled switched trunk port to handle multiple vlans that support DHCP, DNS, and a wireless network.


It is important to take note of the physical port mappings on the GL-iNet ATX-1800 Wireless Router. This is how the internal logic of ports on the CPU-based software router routes to the physical ports and ethernet configurations.

___
####
WAN = eth0  
Port 3 = LAN2 = eth1
Port 4 = LAN1 = eth2
####
____

For this setup, WAN (eth0) is the physical ethernet you can connect to ANY IP-based network to get internet.
LAN1 is the trunk port that will only work if plugged into another switch trunk port that is VLAN aware and in this case of VLAN2 and VLAN4.

###
___
VLAN4 - Control Network - 192.168.8.15/24
VLAN2 - NDI Network - 10.155.150.15/24

LAN2 Port on the router is the default backdoor should you need to gain quick access into the Router that is on a non-vlan subnet of 192.168.9.1/24

____
####
/etc/firewall.user
This file is a needed script that needs to run needed iptable rules to run and stay persistent on every reboot:
####
____




_____
#####  Base ifconfig of the network interfaces:

br-vlan4  Link encap:Ethernet  HWaddr AA:E0:C3:75:20:AE  
          inet addr:192.168.8.15  Bcast:192.168.8.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1967 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2937 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:580728 (567.1 KiB)  TX bytes:898215 (877.1 KiB)

eth0      Link encap:Ethernet  HWaddr 94:83:C4:45:39:44  
          inet addr:192.168.100.35  Bcast:192.168.100.255  Mask:255.255.255.0
          inet6 addr: fe80::9683:c4ff:fe45:3944/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:54268 errors:0 dropped:52 overruns:0 frame:0
          TX packets:23911 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:24889985 (23.7 MiB)  TX bytes:6435686 (6.1 MiB)
          Base address:0x1000 

eth2      Link encap:Ethernet  HWaddr AA:E0:C3:75:20:AE  
          inet6 addr: fe80::a8e0:c3ff:fe75:20ae/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:17540 errors:0 dropped:45 overruns:0 frame:0
          TX packets:28554 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:5766474 (5.4 MiB)  TX bytes:21044566 (20.0 MiB)
          Base address:0x1400 

eth2.2    Link encap:Ethernet  HWaddr AA:E0:C3:75:20:AE  
          inet addr:10.155.150.15  Bcast:10.155.150.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:16389 errors:0 dropped:0 overruns:0 frame:0
          TX packets:20700 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:5118784 (4.8 MiB)  TX bytes:19854335 (18.9 MiB)

eth2.4    Link encap:Ethernet  HWaddr AA:E0:C3:75:20:AE  
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:766 errors:0 dropped:0 overruns:0 frame:0
          TX packets:1692 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:233780 (228.3 KiB)  TX bytes:540215 (527.5 KiB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:54 errors:0 dropped:0 overruns:0 frame:0
          TX packets:54 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1 
          RX bytes:7489 (7.3 KiB)  TX bytes:7489 (7.3 KiB)

wlan0     Link encap:Ethernet  HWaddr FA:79:2F:92:16:AF  
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1271 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2258 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:4096 
          RX bytes:368673 (360.0 KiB)  TX bytes:434920 (424.7 KiB)

wlan1     Link encap:Ethernet  HWaddr 0E:30:A3:AF:B0:60  
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:1052 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:4096 
          RX bytes:0 (0.0 B)  TX bytes:85452 (83.4 KiB)
____
End of ifconfig
