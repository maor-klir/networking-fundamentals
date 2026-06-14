# Deliverables

## Routing table before BGP (only local routes) and after (local + bgp routes)

- Before BGP (showing only local routes)

```sh
--{ running }--[  ]--
A:srl1# show network-instance default route-table ipv4-unicast summary
--------------------------------------------------------------------------------------------------------------------------------------------
IPv4 unicast route table of network instance default
--------------------------------------------------------------------------------------------------------------------------------------------
+--------------+------+----------+-------------------+---------+---------+--------+----------+---------+---------+---------+------------+
|    Prefix    |  ID  |  Route   |    Route Owner    | Active  | Origin  | Metric |   Pref   |  Next-  |  Next-  | Backup  |   Backup   |
|              |      |   Type   |                   |         | Network |        |          |   hop   | hop Int |  Next-  |  Next-hop  |
|              |      |          |                   |         | Instanc |        |          | (Type)  | erface  |   hop   | Interface  |
|              |      |          |                   |         |    e    |        |          |         |         | (Type)  |            |
+==============+======+==========+===================+=========+=========+========+==========+=========+=========+=========+============+
| 10.1.1.0/24  | 2    | local    | net_inst_mgr      | True    | default | 0      | 0        | 10.1.1. | etherne |         |            |
|              |      |          |                   |         |         |        |          | 1 (dire | t-1/1.0 |         |            |
|              |      |          |                   |         |         |        |          | ct)     |         |         |            |
| 10.1.1.1/32  | 2    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (e | None    |         |            |
|              |      |          |                   |         |         |        |          | xtract) |         |         |            |
| 10.1.1.255/3 | 2    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (b |         |         |            |
| 2            |      |          |                   |         |         |        |          | roadcas |         |         |            |
|              |      |          |                   |         |         |        |          | t)      |         |         |            |
| 10.1.2.0/24  | 3    | local    | net_inst_mgr      | True    | default | 0      | 0        | 10.1.2. | etherne |         |            |
|              |      |          |                   |         |         |        |          | 1 (dire | t-1/2.0 |         |            |
|              |      |          |                   |         |         |        |          | ct)     |         |         |            |
| 10.1.2.1/32  | 3    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (e | None    |         |            |
|              |      |          |                   |         |         |        |          | xtract) |         |         |            |
| 10.1.2.255/3 | 3    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (b |         |         |            |
| 2            |      |          |                   |         |         |        |          | roadcas |         |         |            |
|              |      |          |                   |         |         |        |          | t)      |         |         |            |
| 10.1.3.0/24  | 4    | local    | net_inst_mgr      | True    | default | 0      | 0        | 10.1.3. | etherne |         |            |
|              |      |          |                   |         |         |        |          | 1 (dire | t-1/3.0 |         |            |
|              |      |          |                   |         |         |        |          | ct)     |         |         |            |
| 10.1.3.1/32  | 4    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (e | None    |         |            |
|              |      |          |                   |         |         |        |          | xtract) |         |         |            |
| 10.1.3.255/3 | 4    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (b |         |         |            |
| 2            |      |          |                   |         |         |        |          | roadcas |         |         |            |
|              |      |          |                   |         |         |        |          | t)      |         |         |            |
+--------------+------+----------+-------------------+---------+---------+--------+----------+---------+---------+---------+------------+
--------------------------------------------------------------------------------------------------------------------------------------------
IPv4 routes total                    : 9
IPv4 prefixes with active routes     : 9
IPv4 prefixes with active ECMP routes: 0
--------------------------------------------------------------------------------------------------------------------------------------------
```

- After BGP (showing local + bgp routes)

```sh
--{ + running }--[  ]--
A:srl1#  show network-instance default route-table ipv4-unicast summary
--------------------------------------------------------------------------------------------------------------------------------------------
IPv4 unicast route table of network instance default
--------------------------------------------------------------------------------------------------------------------------------------------
+--------------+------+----------+-------------------+---------+---------+--------+----------+---------+---------+---------+------------+
|    Prefix    |  ID  |  Route   |    Route Owner    | Active  | Origin  | Metric |   Pref   |  Next-  |  Next-  | Backup  |   Backup   |
|              |      |   Type   |                   |         | Network |        |          |   hop   | hop Int |  Next-  |  Next-hop  |
|              |      |          |                   |         | Instanc |        |          | (Type)  | erface  |   hop   | Interface  |
|              |      |          |                   |         |    e    |        |          |         |         | (Type)  |            |
+==============+======+==========+===================+=========+=========+========+==========+=========+=========+=========+============+
| 10.1.1.0/24  | 2    | local    | net_inst_mgr      | True    | default | 0      | 0        | 10.1.1. | etherne |         |            |
|              |      |          |                   |         |         |        |          | 1 (dire | t-1/1.0 |         |            |
|              |      |          |                   |         |         |        |          | ct)     |         |         |            |
| 10.1.1.1/32  | 2    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (e | None    |         |            |
|              |      |          |                   |         |         |        |          | xtract) |         |         |            |
| 10.1.1.255/3 | 2    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (b |         |         |            |
| 2            |      |          |                   |         |         |        |          | roadcas |         |         |            |
|              |      |          |                   |         |         |        |          | t)      |         |         |            |
| 10.1.2.0/24  | 3    | local    | net_inst_mgr      | True    | default | 0      | 0        | 10.1.2. | etherne |         |            |
|              |      |          |                   |         |         |        |          | 1 (dire | t-1/2.0 |         |            |
|              |      |          |                   |         |         |        |          | ct)     |         |         |            |
| 10.1.2.1/32  | 3    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (e | None    |         |            |
|              |      |          |                   |         |         |        |          | xtract) |         |         |            |
| 10.1.2.255/3 | 3    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (b |         |         |            |
| 2            |      |          |                   |         |         |        |          | roadcas |         |         |            |
|              |      |          |                   |         |         |        |          | t)      |         |         |            |
| 10.1.3.0/24  | 4    | local    | net_inst_mgr      | True    | default | 0      | 0        | 10.1.3. | etherne |         |            |
|              |      |          |                   |         |         |        |          | 1 (dire | t-1/3.0 |         |            |
|              |      |          |                   |         |         |        |          | ct)     |         |         |            |
| 10.1.3.1/32  | 4    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (e | None    |         |            |
|              |      |          |                   |         |         |        |          | xtract) |         |         |            |
| 10.1.3.255/3 | 4    | host     | net_inst_mgr      | True    | default | 0      | 0        | None (b |         |         |            |
| 2            |      |          |                   |         |         |        |          | roadcas |         |         |            |
|              |      |          |                   |         |         |        |          | t)      |         |         |            |
| 10.1.4.0/24  | 0    | bgp      | bgp_mgr           | True    | default | 0      | 170      | 10.1.2. | etherne |         |            |
|              |      |          |                   |         |         |        |          | 0/24 (i | t-1/2.0 |         |            |
|              |      |          |                   |         |         |        |          | ndirect |         |         |            |
|              |      |          |                   |         |         |        |          | /local) |         |         |            |
| 10.1.5.0/24  | 0    | bgp      | bgp_mgr           | True    | default | 0      | 170      | 10.1.3. | etherne |         |            |
|              |      |          |                   |         |         |        |          | 0/24 (i | t-1/3.0 |         |            |
|              |      |          |                   |         |         |        |          | ndirect |         |         |            |
|              |      |          |                   |         |         |        |          | /local) |         |         |            |
+--------------+------+----------+-------------------+---------+---------+--------+----------+---------+---------+---------+------------+
--------------------------------------------------------------------------------------------------------------------------------------------
IPv4 routes total                    : 11
IPv4 prefixes with active routes     : 11
IPv4 prefixes with active ECMP routes: 0
--------------------------------------------------------------------------------------------------------------------------------------------
```

## All cross-subnet pings succeeding

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​ 2s
❯ docker exec clab-dynamic-routing-bgp-host1 ping -c 3 10.1.4.2
PING 10.1.4.2 (10.1.4.2) 56(84) bytes of data.
64 bytes from 10.1.4.2: icmp_seq=1 ttl=62 time=0.283 ms
64 bytes from 10.1.4.2: icmp_seq=2 ttl=62 time=0.416 ms
64 bytes from 10.1.4.2: icmp_seq=3 ttl=62 time=0.402 ms

--- 10.1.4.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2049ms
rtt min/avg/max/mdev = 0.283/0.367/0.416/0.059 ms

~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​ 2s
❯ docker exec clab-dynamic-routing-bgp-host1 ping -c 3 10.1.5.2
PING 10.1.5.2 (10.1.5.2) 56(84) bytes of data.
64 bytes from 10.1.5.2: icmp_seq=1 ttl=62 time=0.338 ms
64 bytes from 10.1.5.2: icmp_seq=2 ttl=62 time=0.550 ms
64 bytes from 10.1.5.2: icmp_seq=3 ttl=62 time=0.474 ms

--- 10.1.5.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2080ms
rtt min/avg/max/mdev = 0.338/0.454/0.550/0.087 ms

~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​ 2s
❯ docker exec clab-dynamic-routing-bgp-host2 ping -c 3 10.1.5.2
PING 10.1.5.2 (10.1.5.2) 56(84) bytes of data.
64 bytes from 10.1.5.2: icmp_seq=1 ttl=61 time=0.450 ms
64 bytes from 10.1.5.2: icmp_seq=2 ttl=61 time=0.591 ms
64 bytes from 10.1.5.2: icmp_seq=3 ttl=61 time=0.652 ms

--- 10.1.5.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2059ms
rtt min/avg/max/mdev = 0.450/0.564/0.652/0.084 ms
```
