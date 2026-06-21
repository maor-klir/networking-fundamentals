# Deliverables

## Routing table showing static type for 10.1.4.0/24 with the wrong next-hop

Both pings succeed because **the srl2-srl3 link from exercise 2 is still installed**:

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​
❯ docker exec clab-dynamic-routing-bgp-host1 ping -c 3 -W 5 10.1.4.2
PING 10.1.4.2 (10.1.4.2) 56(84) bytes of data.
64 bytes from 10.1.4.2: icmp_seq=1 ttl=62 time=0.436 ms
64 bytes from 10.1.4.2: icmp_seq=2 ttl=62 time=0.501 ms
64 bytes from 10.1.4.2: icmp_seq=3 ttl=62 time=0.510 ms

--- 10.1.4.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2082ms
rtt min/avg/max/mdev = 0.436/0.482/0.510/0.032 ms

~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​ 2s
❯ docker exec clab-dynamic-routing-bgp-host1 ping -c 3 10.1.5.2
PING 10.1.5.2 (10.1.5.2) 56(84) bytes of data.
64 bytes from 10.1.5.2: icmp_seq=1 ttl=62 time=0.431 ms
64 bytes from 10.1.5.2: icmp_seq=2 ttl=62 time=0.382 ms
64 bytes from 10.1.5.2: icmp_seq=3 ttl=62 time=0.384 ms

--- 10.1.5.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2052ms
rtt min/avg/max/mdev = 0.382/0.399/0.431/0.022 ms
```

- On srl1:

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​ 2s
❯ docker exec -it clab-dynamic-routing-bgp-srl1 sr_cli -c "show network-instance default route-table ipv4-unicast summary"
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 unicast route table of network instance default
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
+--------------------------------------------+-------+------------+----------------------+----------+----------+---------+------------+---------------------------+---------------------------+---------------------------+--------------------------------------------------------------+
|                   Prefix                   |  ID   | Route Type |     Route Owner      |  Active  |  Origin  | Metric  |    Pref    |      Next-hop (Type)      |    Next-hop Interface     |  Backup Next-hop (Type)   |                  Backup Next-hop Interface                   |
|                                            |       |            |                      |          | Network  |         |            |                           |                           |                           |                                                              |
|                                            |       |            |                      |          | Instance |         |            |                           |                           |                           |                                                              |
+============================================+=======+============+======================+==========+==========+=========+============+===========================+===========================+===========================+==============================================================+
| 10.1.1.0/24                                | 2     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.1.1 (direct)         | ethernet-1/1.0            |                           |                                                              |
| 10.1.1.1/32                                | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)            | None                      |                           |                                                              |
| 10.1.1.255/32                              | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast)          |                           |                           |                                                              |
| 10.1.2.0/24                                | 3     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.2.1 (direct)         | ethernet-1/2.0            |                           |                                                              |
| 10.1.2.1/32                                | 3     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)            | None                      |                           |                                                              |
| 10.1.2.255/32                              | 3     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast)          |                           |                           |                                                              |
| 10.1.3.0/24                                | 4     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.3.1 (direct)         | ethernet-1/3.0            |                           |                                                              |
| 10.1.3.1/32                                | 4     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)            | None                      |                           |                                                              |
| 10.1.3.255/32                              | 4     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast)          |                           |                           |                                                              |
| 10.1.4.0/24                                | 0     | static     | static_route_mgr     | True     | default  | 1       | 5          | 10.1.3.0/24               | ethernet-1/3.0            |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
| 10.1.5.0/24                                | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.1.3.0/24               | ethernet-1/3.0            |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
| 10.1.6.0/24                                | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.1.2.0/24               | ethernet-1/2.0            |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
+--------------------------------------------+-------+------------+----------------------+----------+----------+---------+------------+---------------------------+---------------------------+---------------------------+--------------------------------------------------------------+
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 routes total                    : 12
IPv4 prefixes with active routes     : 12
IPv4 prefixes with active ECMP routes: 0
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```
- On srl3:

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​
❯ docker exec -it clab-dynamic-routing-bgp-srl3 sr_cli -c \
  "show network-instance default route-table ipv4-unicast summary"
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 unicast route table of network instance default
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
+--------------------------------------------+-------+------------+----------------------+----------+----------+---------+------------+---------------------------+---------------------------+---------------------------+--------------------------------------------------------------+
|                   Prefix                   |  ID   | Route Type |     Route Owner      |  Active  |  Origin  | Metric  |    Pref    |      Next-hop (Type)      |    Next-hop Interface     |  Backup Next-hop (Type)   |                  Backup Next-hop Interface                   |
|                                            |       |            |                      |          | Network  |         |            |                           |                           |                           |                                                              |
|                                            |       |            |                      |          | Instance |         |            |                           |                           |                           |                                                              |
+============================================+=======+============+======================+==========+==========+=========+============+===========================+===========================+===========================+==============================================================+
| 10.1.1.0/24                                | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.1.3.0/24               | ethernet-1/1.0            |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
| 10.1.2.0/24                                | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.1.3.0/24               | ethernet-1/1.0            |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
| 10.1.3.0/24                                | 2     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.3.2 (direct)         | ethernet-1/1.0            |                           |                                                              |
| 10.1.3.2/32                                | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)            | None                      |                           |                                                              |
| 10.1.3.255/32                              | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast)          |                           |                           |                                                              |
| 10.1.4.0/24                                | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.1.6.0/24               | ethernet-1/3.0            |                           |                                                              |
|                                            |       |            |                      |          |          |         |            | (indirect/local)          |                           |                           |                                                              |
| 10.1.5.0/24                                | 3     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.5.1 (direct)         | ethernet-1/2.0            |                           |                                                              |
| 10.1.5.1/32                                | 3     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)            | None                      |                           |                                                              |
| 10.1.5.255/32                              | 3     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast)          |                           |                           |                                                              |
| 10.1.6.0/24                                | 4     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.6.2 (direct)         | ethernet-1/3.0            |                           |                                                              |
| 10.1.6.2/32                                | 4     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)            | None                      |                           |                                                              |
| 10.1.6.255/32                              | 4     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast)          |                           |                           |                                                              |
+--------------------------------------------+-------+------------+----------------------+----------+----------+---------+------------+---------------------------+---------------------------+---------------------------+--------------------------------------------------------------+
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 routes total                    : 12
IPv4 prefixes with active routes     : 12
IPv4 prefixes with active ECMP routes: 0
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```

srl3 has a BGP route for host2 via the direct srl2-srl3 link from exercise 2:

1. host1 -> srl1 -> srl3 (wrong direction, but srl3 accepts it)
2. srl3 -> srl2 -> host2 (srl3 forwards it correctly via its own BGP route)
3. Reply traffic returns normally
The packet makes it through via an unintended detour, which is exactly why both pings succeed.  
To completely break traffic from host1 to host2, the direct srl2-srl3 link from exercise 2 must be removed.

## Explanation of why static routes (admin distance 5) beat BGP routes (admin distance 170)

`Pref` is the administrative distance: a trust level score assigned to the routing protocol that learned the route. Lower wins.  
When srl1 has both a static route and a BGP route for the same prefix (10.1.4.0/24), it installs the static route into the forwarding table and completely ignores the BGP route, not because the BGP route is wrong, but purely because 5 < 170.  
BGP still knows the correct route. BGP just lost the tiebreaker before the route ever made it into the active forwarding table.  
This is why stale static routes are dangerous in production. BGP can be perfectly healthy, sessions established, correct routes received and traffic still blackholes because a forgotten static route from years ago silently wins on administrative distance.
