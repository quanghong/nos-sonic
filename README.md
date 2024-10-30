# Learning Software for Open Networking in the Cloud (SONiC) 
https://sonicfoundation.dev/

## Images
* Download: https://sonic.software/
* Broadcom virtual switch: version 3.1.2
* Account: admin/YourPaSsWoRd
* Interface: format with file sonicsw.yml
```bash
cp ./EVE-NG/sonicsw-broadcom.yml /opt/unetlab/html/templates/intel/sonicsw.yml
```

## Maintainance Mode
We have 3 options to maintain network services without affect to traffic loading.
* <b>Docker container maintainance</b>
    * Maintain each container without affect to others. Like VRRP container, DHCP container.
* <b>Network OS maintainance</b>
    * Maintain whole OS running on device - Control Plane. But it still don't affect to Data Plane.
* <b>Maintaince mode</b>
    * Graceful isolation of Swich on forwarding path.
    * Maintainance on switch without affect to user traffic.

## Basic configuration
**Disable ZTP**
```bash
sudo ztp disable -y
```
**Configuration mode**
```bash
sonic-cli
```

## BGP Underlay
```bash
Spine-2# show bgp ipv4 unicast summary
BGP router identifier 192.168.0.2, local AS number 65000
Neighbor    V   AS      MsgRcvd   MsgSent   InQ     OutQ    Up/Down         State/PfxRcd
1.1.1.0     4   65101   8         11        0       0       00:00:27        2
1.1.1.3     4   65102   8         11        0       0       00:00:25        2
Total number of neighbors 2
Total number of neighbors established 2

Leaf-2# show ip route
Codes:  K - kernel route, C - connected, S - static, B - BGP, O - OSPF
        > - selected route, * - FIB route, q - queued route, r - rejected route, # - not installed in hardware
       Destination                  Gateway                                                Dist/Metric   Uptime
-------------------------------------------------------------------------------------------------------------------
 B>*   1.1.1.0/31                   via 1.1.1.2                   Ethernet4                20/0          00:01:47
 C>*   1.1.1.2/31                   Direct                        Ethernet4                0/0           00:26:05
 B>*   192.168.0.1/32               via 1.1.1.2                   Ethernet4                20/0          00:01:47
 B>*   192.168.0.2/32               via 1.1.1.2                   Ethernet4                20/0          00:01:47
 C>*   192.168.0.3/32               Direct                        Loopback0                0/0           00:26:15

Leaf-2# ping 192.168.0.1
PING 192.168.0.1 (192.168.0.1) 56(84) bytes of data.
64 bytes from 192.168.0.1: icmp_seq=1 ttl=63 time=12.9 ms
64 bytes from 192.168.0.1: icmp_seq=2 ttl=63 time=20.9 ms
64 bytes from 192.168.0.1: icmp_seq=3 ttl=63 time=7.81 ms
64 bytes from 192.168.0.1: icmp_seq=4 ttl=63 time=12.5 ms
64 bytes from 192.168.0.1: icmp_seq=5 ttl=63 time=6.49 ms
```

# OSPF 
point-to-point network type
* No election, reducing sending multicast message to elect DR, BDR.
* Minize LSDB and faster network convergence, only exchange LSA Type 1, no Type 2.