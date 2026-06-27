# Deliverables

## Ping output showing brief loss then recovery

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/05-spine-leaf-bgp main*​ 9s
❯ docker exec clab-spine-leaf-bgp-host1 ping -c 1000 -i 1 10.20.4.2
PING 10.20.4.2 (10.20.4.2) 56(84) bytes of data.
64 bytes from 10.20.4.2: icmp_seq=1 ttl=61 time=0.557 ms
64 bytes from 10.20.4.2: icmp_seq=2 ttl=61 time=0.350 ms
64 bytes from 10.20.4.2: icmp_seq=3 ttl=61 time=0.341 ms
64 bytes from 10.20.4.2: icmp_seq=4 ttl=61 time=0.335 ms
64 bytes from 10.20.4.2: icmp_seq=5 ttl=61 time=0.328 ms
64 bytes from 10.20.4.2: icmp_seq=6 ttl=61 time=0.335 ms
64 bytes from 10.20.4.2: icmp_seq=7 ttl=61 time=0.347 ms
64 bytes from 10.20.4.2: icmp_seq=8 ttl=61 time=0.283 ms
64 bytes from 10.20.4.2: icmp_seq=9 ttl=61 time=0.320 ms
64 bytes from 10.20.4.2: icmp_seq=10 ttl=61 time=0.323 ms
.
.
.
64 bytes from 10.20.4.2: icmp_seq=43 ttl=61 time=0.319 ms
64 bytes from 10.20.4.2: icmp_seq=44 ttl=61 time=0.328 ms
64 bytes from 10.20.4.2: icmp_seq=45 ttl=61 time=0.284 ms
64 bytes from 10.20.4.2: icmp_seq=46 ttl=61 time=0.288 ms
64 bytes from 10.20.4.2: icmp_seq=47 ttl=61 time=0.310 ms
64 bytes from 10.20.4.2: icmp_seq=48 ttl=61 time=0.338 ms
64 bytes from 10.20.4.2: icmp_seq=49 ttl=61 time=0.328 ms
64 bytes from 10.20.4.2: icmp_seq=50 ttl=61 time=0.325 ms
64 bytes from 10.20.4.2: icmp_seq=51 ttl=61 time=0.325 ms
64 bytes from 10.20.4.2: icmp_seq=52 ttl=61 time=0.330 ms
64 bytes from 10.20.4.2: icmp_seq=53 ttl=61 time=0.343 ms
64 bytes from 10.20.4.2: icmp_seq=54 ttl=61 time=0.361 ms
64 bytes from 10.20.4.2: icmp_seq=55 ttl=61 time=0.335 ms
64 bytes from 10.20.4.2: icmp_seq=56 ttl=61 time=0.410 ms
64 bytes from 10.20.4.2: icmp_seq=57 ttl=61 time=0.500 ms
64 bytes from 10.20.4.2: icmp_seq=58 ttl=61 time=0.416 ms
64 bytes from 10.20.4.2: icmp_seq=59 ttl=61 time=1.37 ms   <--- nearly a packet drop
64 bytes from 10.20.4.2: icmp_seq=60 ttl=61 time=0.424 ms
64 bytes from 10.20.4.2: icmp_seq=61 ttl=61 time=0.424 ms
64 bytes from 10.20.4.2: icmp_seq=62 ttl=61 time=0.323 ms
64 bytes from 10.20.4.2: icmp_seq=63 ttl=61 time=0.334 ms
.
.
.

```

## Routing table before/after showing ECMP path count change (2 -> 1 -> 2)

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/05-spine-leaf-bgp main*
❯ docker exec -it clab-spine-leaf-bgp-leaf1 sr_cli -c "show network-instance default route-table ipv4-unicast summary"
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 unicast route table of network instance default
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
+--------------------------------------------+-------+------------+----------------------+----------+----------+---------+------------+---------------------------+---------------------------+---------------------------+--------------------------------------------------------------+
|                   Prefix                   |  ID   | Route Type |     Route Owner      |  Active  |  Origin  | Metric  |    Pref    |      Next-hop (Type)      |    Next-hop Interface     |  Backup Next-hop (Type)   |                  Backup Next-hop Interface                   |
|                                            |       |            |                      |          | Network  |         |            |                           |                           |                           |                                                              |
|                                            |       |            |                      |          | Instance |         |            |                           |                           |                           |                                                              |
+============================================+=======+============+======================+==========+==========+=========+============+===========================+===========================+===========================+==============================================================+
| 10.10.1.0/31                               | 3     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.10.1.1 (direct)        | ethernet-1/49.0           |                           |                                                              |
| 10.10.1.1/32                               | 3     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)            | None                      |                           |                                                              |
| 10.10.2.0/31                               | 4     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.10.2.1 (direct)        | ethernet-1/50.0           |                           |                                                              |
| 10.10.2.1/32                               | 4     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)            | None                      |                           |                                                              |
| 10.20.1.0/24                               | 2     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.20.1.1 (direct)        | ethernet-1/1.0            |                           |                                                              |
| 10.20.1.1/32                               | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)            | None                      |                           |                                                              |
| 10.20.1.255/32                             | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast)          |                           |                           |                                                              |
| 10.20.2.0/24                               | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.10.1.0/31              | ethernet-1/49.0           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          | ethernet-1/50.0           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | 10.10.2.0/31              |                           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
| 10.20.3.0/24                               | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.10.1.0/31              | ethernet-1/49.0           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          | ethernet-1/50.0           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | 10.10.2.0/31              |                           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
| 10.20.4.0/24                               | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.10.1.0/31              | ethernet-1/49.0           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          | ethernet-1/50.0           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | 10.10.2.0/31              |                           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
+--------------------------------------------+-------+------------+----------------------+----------+----------+---------+------------+---------------------------+---------------------------+---------------------------+--------------------------------------------------------------+
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 routes total                    : 10
IPv4 prefixes with active routes     : 10
IPv4 prefixes with active ECMP routes: 3
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

~/repositories/github/maor-klir/network-fundamentals/lessons/clab/05-spine-leaf-bgp main*
❯ docker exec -it clab-spine-leaf-bgp-spine1 sr_cli
Using configuration file(s): []
Welcome to the srlinux CLI.
Type 'help' (and press <ENTER>) if you need any help using this.
--{ + running }--[  ]--
A:spine1# enter candidate
set / interface ethernet-1/1 admin-state disable
set / interface ethernet-1/2 admin-state disable
set / interface ethernet-1/3 admin-state disable
set / interface ethernet-1/4 admin-state disable
commit now
exit
All changes have been committed. Leaving candidate mode.
--{ + running }--[  ]--
A:spine1#
EOF encountered

~/repositories/github/maor-klir/network-fundamentals/lessons/clab/05-spine-leaf-bgp main* 8s
❯ docker exec -it clab-spine-leaf-bgp-leaf1 sr_cli -c "show network-instance default route-table ipv4-unicast summary"
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 unicast route table of network instance default
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
+--------------------------------------------+-------+------------+----------------------+----------+----------+---------+------------+---------------------------+---------------------------+---------------------------+--------------------------------------------------------------+
|                   Prefix                   |  ID   | Route Type |     Route Owner      |  Active  |  Origin  | Metric  |    Pref    |      Next-hop (Type)      |    Next-hop Interface     |  Backup Next-hop (Type)   |                  Backup Next-hop Interface                   |
|                                            |       |            |                      |          | Network  |         |            |                           |                           |                           |                                                              |
|                                            |       |            |                      |          | Instance |         |            |                           |                           |                           |                                                              |
+============================================+=======+============+======================+==========+==========+=========+============+===========================+===========================+===========================+==============================================================+
| 10.10.2.0/31                               | 4     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.10.2.1 (direct)        | ethernet-1/50.0           |                           |                                                              |
| 10.10.2.1/32                               | 4     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)            | None                      |                           |                                                              |
| 10.20.1.0/24                               | 2     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.20.1.1 (direct)        | ethernet-1/1.0            |                           |                                                              |
| 10.20.1.1/32                               | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)            | None                      |                           |                                                              |
| 10.20.1.255/32                             | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast)          |                           |                           |                                                              |
| 10.20.2.0/24                               | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.10.2.0/31              | ethernet-1/50.0           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
| 10.20.3.0/24                               | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.10.2.0/31              | ethernet-1/50.0           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
| 10.20.4.0/24                               | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.10.2.0/31              | ethernet-1/50.0           |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
+--------------------------------------------+-------+------------+----------------------+----------+----------+---------+------------+---------------------------+---------------------------+---------------------------+--------------------------------------------------------------+
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 routes total                    : 8
IPv4 prefixes with active routes     : 8
IPv4 prefixes with active ECMP routes: 0
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```

## Explanation of fabric resilience: why losing a spine degrades capacity but not connectivity

In a Clos spine-leaf fabric, every leaf connects to every spine, ensuring there is always more than one path between any two leaves.  
When a spine fails, BGP detects the loss, withdraws its routes, and each leaf automatically reroutes traffic through the remaining spine.  
Full connectivity is preserved with only a brief reconvergence disruption.  

However, capacity is degraded because the fabric drops from 2 ECMP paths per destination to 1, cutting aggregate east-west bandwidth roughly in half.  
This graceful degradation: connectivity preserved, capacity reduced, is the core value of the Clos design, where redundancy is structural rather than manually configured.
