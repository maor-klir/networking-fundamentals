# Deliverables

## BGP neighbor output from a leaf (showing 2 established sessions)

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/05-spine-leaf-bgp main*
❯ docker exec -it clab-spine-leaf-bgp-leaf3 sr_cli -c "show network-instance default protocols bgp neighbor"
--------------------------------------------------------------------------------------------------------------------------------------------
BGP neighbor summary for network-instance "default"
Flags: S static, D dynamic, L discovered by LLDP, B BFD enabled, - disabled, * slow
--------------------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------------------
+----------------+-----------------------+----------------+------+---------+-------------+-------------+-----------+-----------------------+
|    Net-Inst    |         Peer          |     Group      | Flag | Peer-AS |    State    |   Uptime    | AFI/SAFI  |    [Rx/Active/Tx]     |
|                |                       |                |  s   |         |             |             |           |                       |
+================+=======================+================+======+=========+=============+=============+===========+=======================+
| default        | 10.10.1.4             | spines         | S    | 65000   | established | 0d:0h:6m:32 | ipv4-     | [3/3/1]               |
|                |                       |                |      |         |             | s           | unicast   |                       |
| default        | 10.10.2.4             | spines         | S    | 65000   | established | 0d:0h:6m:30 | ipv4-     | [3/3/4]               |
|                |                       |                |      |         |             | s           | unicast   |                       |
+----------------+-----------------------+----------------+------+---------+-------------+-------------+-----------+-----------------------+
--------------------------------------------------------------------------------------------------------------------------------------------
Summary:
2 configured neighbors, 2 configured sessions are established, 0 disabled peers
0 dynamic peers
```

## BGP neighbor output from a spine (showing 4 established sessions)

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/05-spine-leaf-bgp main*
❯ docker exec -it clab-spine-leaf-bgp-spine2 sr_cli -c "show network-instance default protocols bgp neighbor"
--------------------------------------------------------------------------------------------------------------------------------------------
BGP neighbor summary for network-instance "default"
Flags: S static, D dynamic, L discovered by LLDP, B BFD enabled, - disabled, * slow
--------------------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------------------
+----------------+-----------------------+----------------+------+---------+-------------+-------------+-----------+-----------------------+
|    Net-Inst    |         Peer          |     Group      | Flag | Peer-AS |    State    |   Uptime    | AFI/SAFI  |    [Rx/Active/Tx]     |
|                |                       |                |  s   |         |             |             |           |                       |
+================+=======================+================+======+=========+=============+=============+===========+=======================+
| default        | 10.10.2.1             | leaves         | S    | 65001   | established | 0d:0h:7m:53 | ipv4-     | [4/1/3]               |
|                |                       |                |      |         |             | s           | unicast   |                       |
| default        | 10.10.2.3             | leaves         | S    | 65002   | established | 0d:0h:7m:53 | ipv4-     | [4/1/3]               |
|                |                       |                |      |         |             | s           | unicast   |                       |
| default        | 10.10.2.5             | leaves         | S    | 65003   | established | 0d:0h:7m:53 | ipv4-     | [4/1/3]               |
|                |                       |                |      |         |             | s           | unicast   |                       |
| default        | 10.10.2.7             | leaves         | S    | 65004   | established | 0d:0h:7m:53 | ipv4-     | [4/1/3]               |
|                |                       |                |      |         |             | s           | unicast   |                       |
+----------------+-----------------------+----------------+------+---------+-------------+-------------+-----------+-----------------------+
--------------------------------------------------------------------------------------------------------------------------------------------
Summary:
4 configured neighbors, 4 configured sessions are established, 0 disabled peers
0 dynamic peers
```
