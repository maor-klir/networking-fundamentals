# Deliverables

## Routing table showing ECMP entries (2 next-hops per remote leaf subnet)

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
```

## `traceroute` output from multiple runs

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/05-spine-leaf-bgp main* 2s
❯ docker exec clab-spine-leaf-bgp-host1 traceroute -n -w 2 10.20.4.2
traceroute to 10.20.4.2 (10.20.4.2), 30 hops max, 60 byte packets
 1  10.20.1.1  1.167 ms  1.145 ms  1.138 ms
 2  10.10.1.0  0.471 ms 10.10.2.0  1.153 ms 10.10.1.0  1.408 ms   <--- through spine1
 3  10.10.2.7  0.727 ms  0.728 ms  0.727 ms
 4  10.20.4.2  0.648 ms *  0.610 ms

~/repositories/github/maor-klir/network-fundamentals/lessons/clab/05-spine-leaf-bgp main* 2s
❯ docker exec clab-spine-leaf-bgp-host1 traceroute -n -w 2 10.20.4.2
traceroute to 10.20.4.2 (10.20.4.2), 30 hops max, 60 byte packets
 1  10.20.1.1  0.569 ms  0.535 ms  0.525 ms
 2  10.10.2.0  1.259 ms 10.10.1.0  0.514 ms 10.10.2.0  1.253 ms   <--- through spine2
 3  10.10.1.7  0.790 ms 10.10.2.7  0.804 ms 10.10.1.7  0.786 ms
 4  10.20.4.2  0.525 ms  0.598 ms  0.529 ms
```

## Answers to the 3 questions above

- How many hops are between any two hosts?
There are always 4 hops between any two hosts. Always following this path: host-leaf-spine-leaf-host.

- How many equal-cost paths exist between any two leaves?
Two ECMP paths exist between any two leaves, one per spine.

- What happens to bandwidth if you add a third spine? (50% more aggregate bandwidth, 3 ECMP paths)
Adding a third spine will add 50% more aggregate bandwith becaude there will be three ECMP paths.
