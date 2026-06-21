# Deliverables

## BGP neighbor output showing non-established state

- Before breaking the setup:

```sh
A:srl2# show network-instance default protocols bgp neighbor
--------------------------------------------------------------------------------------------------------------------------------------------
BGP neighbor summary for network-instance "default"
Flags: S static, D dynamic, L discovered by LLDP, B BFD enabled, - disabled, * slow
--------------------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------------------
+----------------+-----------------------+----------------+------+---------+-------------+-------------+-----------+-----------------------+
|    Net-Inst    |         Peer          |     Group      | Flag | Peer-AS |    State    |   Uptime    | AFI/SAFI  |    [Rx/Active/Tx]     |
|                |                       |                |  s   |         |             |             |           |                       |
+================+=======================+================+======+=========+=============+=============+===========+=======================+
| default        | 10.1.2.1              | ebgp-peers     | S    | 65001   | established | 0d:1h:36m:1 | ipv4-     | [4/2/4]               |
|                |                       |                |      |         |             | s           | unicast   |                       |
| default        | 10.1.6.2              | ebgp-peers     | S    | 65003   | established | 0d:1h:35m:2 | ipv4-     | [5/1/5]               |
|                |                       |                |      |         |             | 6s          | unicast   |                       |
+----------------+-----------------------+----------------+------+---------+-------------+-------------+-----------+-----------------------+
--------------------------------------------------------------------------------------------------------------------------------------------
Summary:
2 configured neighbors, 2 configured sessions are established, 0 disabled peers
0 dynamic peers
--{ + running }--[  ]--
```

- Breaking the setup and the output afterwards:

```
A:srl2# enter candidate
set / network-instance default protocols bgp neighbor 10.1.2.1 peer-as 65099
commit now
exit
All changes have been committed. Leaving candidate mode.
--{ + running }--[  ]--
A:srl2# show network-instance default protocols bgp neighbor
--------------------------------------------------------------------------------------------------------------------------------------------
BGP neighbor summary for network-instance "default"
Flags: S static, D dynamic, L discovered by LLDP, B BFD enabled, - disabled, * slow
--------------------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------------------
+----------------+-----------------------+----------------+------+---------+-------------+-------------+-----------+-----------------------+
|    Net-Inst    |         Peer          |     Group      | Flag | Peer-AS |    State    |   Uptime    | AFI/SAFI  |    [Rx/Active/Tx]     |
|                |                       |                |  s   |         |             |             |           |                       |
+================+=======================+================+======+=========+=============+=============+===========+=======================+
| default        | 10.1.2.1              | ebgp-peers     | S    | 65099   | active      | -           |           |                       |
| default        | 10.1.6.2              | ebgp-peers     | S    | 65003   | established | 0d:1h:35m:5 | ipv4-     | [5/3/3]               |
|                |                       |                |      |         |             | 5s          | unicast   |                       |
+----------------+-----------------------+----------------+------+---------+-------------+-------------+-----------+-----------------------+
--------------------------------------------------------------------------------------------------------------------------------------------
Summary:
2 configured neighbors, 1 configured sessions are established, 0 disabled peers
0 dynamic peers
```
srl1 view:

```sh
A:srl1# show network-instance default protocols bgp neighbor
--------------------------------------------------------------------------------------------------------------------------------------------
BGP neighbor summary for network-instance "default"
Flags: S static, D dynamic, L discovered by LLDP, B BFD enabled, - disabled, * slow
--------------------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------------------
+----------------+-----------------------+----------------+------+---------+-------------+-------------+-----------+-----------------------+
|    Net-Inst    |         Peer          |     Group      | Flag | Peer-AS |    State    |   Uptime    | AFI/SAFI  |    [Rx/Active/Tx]     |
|                |                       |                |  s   |         |             |             |           |                       |
+================+=======================+================+======+=========+=============+=============+===========+=======================+
| default        | 10.1.2.2              | ebgp-peers     | S    | 65002   | active      | -           |           |                       |
| default        | 10.1.3.2              | ebgp-peers     | S    | 65003   | established | 0d:1h:35m:3 | ipv4-     | [4/3/3]               |
|                |                       |                |      |         |             | 0s          | unicast   |                       |
+----------------+-----------------------+----------------+------+---------+-------------+-------------+-----------+-----------------------+
--------------------------------------------------------------------------------------------------------------------------------------------
Summary:
2 configured neighbors, 1 configured sessions are established, 0 disabled peers
0 dynamic peers
--{ + running }--[  ]--
```
The long route is being taken:

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​ 3m16s
❯ docker exec clab-dynamic-routing-bgp-host2 ping -c 3 -W 5 10.1.1.2
PING 10.1.1.2 (10.1.1.2) 56(84) bytes of data.
64 bytes from 10.1.1.2: icmp_seq=1 ttl=61 time=0.496 ms
64 bytes from 10.1.1.2: icmp_seq=2 ttl=61 time=0.595 ms
64 bytes from 10.1.1.2: icmp_seq=3 ttl=61 time=0.728 ms

--- 10.1.1.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2050ms
rtt min/avg/max/mdev = 0.496/0.606/0.728/0.095 ms

~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​ 2s
❯ docker exec clab-dynamic-routing-bgp-host2 traceroute -n -w 2 10.1.1.2
traceroute to 10.1.1.2 (10.1.1.2), 30 hops max, 60 byte packets
 1  10.1.4.1  0.743 ms  0.631 ms  1.472 ms
 2  10.1.6.2  1.171 ms  1.181 ms  1.181 ms
 3  10.1.3.1  1.579 ms  1.640 ms  1.908 ms
 4  10.1.1.2  1.351 ms  1.386 ms  1.386 ms
```

## Explanation of AS mismatch

In BGP, every router belongs to an AS, identified by a number.  
When two BGP peers connect, each side must be configured with the correct ASN of its neighbor (peer-as). This is a mutual handshake. Both sides must agree.  
BGP established requires exact ASN agreement on both sides. It's a deliberate security mechanism. You shouldn't be peering with a router you didn't intend to peer with.  
If a session is stuck in `active` or `connect`, that points to a definite error.  
Specifically, the `active` state suggests that the TCP connection succeeds but the `OPEN` message is rejected, which points to an AS number mismatch.  

Another important gotcha that in a real production environment can cause disruption is firewall rules.  
BGP runs entirely over TCP port 179. Before any AS negotiation or route exchange can happen, a plain TCP connection must be established between peers.  
A firewall dropping port 179 kills BGP at the transport layer before the protocol even gets started.
Hence:
- It is crucial to allow outbound port 179 and not forget inbound port 179.
- BGP is bidirectional: one side initiates TCP (source port = ephemeral, dest port = 179), but the other side also needs to be able to accept it.
