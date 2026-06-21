# Deliverables

## Ping output showing loss then recovery

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*<200b> 46s
❯ docker exec clab-dynamic-routing-bgp-host1 ping -c 90 -i 1 10.1.5.2
PING 10.1.5.2 (10.1.5.2) 56(84) bytes of data.
64 bytes from 10.1.5.2: icmp_seq=1 ttl=62 time=1.81 ms
64 bytes from 10.1.5.2: icmp_seq=2 ttl=62 time=0.417 ms
64 bytes from 10.1.5.2: icmp_seq=3 ttl=62 time=0.436 ms
64 bytes from 10.1.5.2: icmp_seq=4 ttl=62 time=0.397 ms
64 bytes from 10.1.5.2: icmp_seq=5 ttl=61 time=0.608 ms     <---- another hop is added, recovery was so quick, no packet loss!
64 bytes from 10.1.5.2: icmp_seq=6 ttl=61 time=0.621 ms
64 bytes from 10.1.5.2: icmp_seq=7 ttl=61 time=0.679 ms
64 bytes from 10.1.5.2: icmp_seq=8 ttl=61 time=0.575 ms
64 bytes from 10.1.5.2: icmp_seq=9 ttl=61 time=0.577 ms
64 bytes from 10.1.5.2: icmp_seq=10 ttl=61 time=0.535 ms
64 bytes from 10.1.5.2: icmp_seq=11 ttl=61 time=0.604 ms
64 bytes from 10.1.5.2: icmp_seq=12 ttl=61 time=0.685 ms
64 bytes from 10.1.5.2: icmp_seq=13 ttl=61 time=0.622 ms
64 bytes from 10.1.5.2: icmp_seq=14 ttl=61 time=0.549 ms
64 bytes from 10.1.5.2: icmp_seq=15 ttl=61 time=0.531 ms

```

## Before/after traceroute

Before fixing:

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*<200b>
❯ docker exec clab-dynamic-routing-bgp-host1 traceroute -n -w 2 10.1.5.2
traceroute to 10.1.5.2 (10.1.5.2), 30 hops max, 60 byte packets
 1  10.1.1.1  2.722 ms  2.663 ms  2.640 ms
 2  10.1.2.2  2.503 ms  2.493 ms  2.480 ms
 3  10.1.6.2  2.475 ms  2.474 ms  2.457 ms
 4  10.1.5.2  0.714 ms  0.801 ms  2.189 ms
```

After fixing:

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*<200b> 35s
❯ docker exec clab-dynamic-routing-bgp-host1 traceroute -n -w 2 10.1.5.2
traceroute to 10.1.5.2 (10.1.5.2), 30 hops max, 60 byte packets
 1  10.1.1.1  1.397 ms  1.365 ms  1.356 ms
 2  10.1.3.2  1.483 ms  1.481 ms  1.517 ms
 3  10.1.5.2  1.281 ms  1.262 ms  1.260 ms
```

Live `ping` output after applying the fix:

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​ 1m31s
❯ docker exec clab-dynamic-routing-bgp-host1 ping -c 90 -i 1 10.1.5.2

PING 10.1.5.2 (10.1.5.2) 56(84) bytes of data.
64 bytes from 10.1.5.2: icmp_seq=1 ttl=61 time=0.786 ms
64 bytes from 10.1.5.2: icmp_seq=2 ttl=61 time=0.619 ms
64 bytes from 10.1.5.2: icmp_seq=3 ttl=61 time=0.543 ms
64 bytes from 10.1.5.2: icmp_seq=4 ttl=61 time=0.594 ms
64 bytes from 10.1.5.2: icmp_seq=5 ttl=61 time=0.584 ms
64 bytes from 10.1.5.2: icmp_seq=6 ttl=61 time=0.656 ms
64 bytes from 10.1.5.2: icmp_seq=7 ttl=61 time=0.607 ms
64 bytes from 10.1.5.2: icmp_seq=8 ttl=61 time=0.546 ms
64 bytes from 10.1.5.2: icmp_seq=9 ttl=61 time=0.635 ms
64 bytes from 10.1.5.2: icmp_seq=10 ttl=61 time=0.591 ms
64 bytes from 10.1.5.2: icmp_seq=11 ttl=61 time=0.604 ms
64 bytes from 10.1.5.2: icmp_seq=12 ttl=61 time=0.580 ms
64 bytes from 10.1.5.2: icmp_seq=13 ttl=61 time=0.600 ms
64 bytes from 10.1.5.2: icmp_seq=14 ttl=61 time=0.592 ms
64 bytes from 10.1.5.2: icmp_seq=15 ttl=61 time=0.551 ms
64 bytes from 10.1.5.2: icmp_seq=16 ttl=61 time=0.556 ms
64 bytes from 10.1.5.2: icmp_seq=17 ttl=61 time=0.579 ms
64 bytes from 10.1.5.2: icmp_seq=18 ttl=61 time=0.627 ms
64 bytes from 10.1.5.2: icmp_seq=19 ttl=61 time=0.538 ms
64 bytes from 10.1.5.2: icmp_seq=20 ttl=61 time=0.534 ms
64 bytes from 10.1.5.2: icmp_seq=21 ttl=61 time=0.554 ms
64 bytes from 10.1.5.2: icmp_seq=22 ttl=61 time=0.582 ms
64 bytes from 10.1.5.2: icmp_seq=23 ttl=61 time=0.625 ms
64 bytes from 10.1.5.2: icmp_seq=24 ttl=61 time=0.572 ms
64 bytes from 10.1.5.2: icmp_seq=25 ttl=61 time=0.556 ms
64 bytes from 10.1.5.2: icmp_seq=26 ttl=61 time=0.560 ms
64 bytes from 10.1.5.2: icmp_seq=27 ttl=61 time=0.568 ms
64 bytes from 10.1.5.2: icmp_seq=28 ttl=61 time=0.654 ms
64 bytes from 10.1.5.2: icmp_seq=29 ttl=61 time=0.660 ms
64 bytes from 10.1.5.2: icmp_seq=30 ttl=61 time=0.609 ms
64 bytes from 10.1.5.2: icmp_seq=31 ttl=61 time=0.602 ms
64 bytes from 10.1.5.2: icmp_seq=32 ttl=61 time=0.623 ms
64 bytes from 10.1.5.2: icmp_seq=33 ttl=61 time=0.564 ms
64 bytes from 10.1.5.2: icmp_seq=34 ttl=61 time=0.587 ms
64 bytes from 10.1.5.2: icmp_seq=35 ttl=61 time=0.590 ms
64 bytes from 10.1.5.2: icmp_seq=36 ttl=61 time=0.600 ms
64 bytes from 10.1.5.2: icmp_seq=37 ttl=61 time=0.558 ms
64 bytes from 10.1.5.2: icmp_seq=38 ttl=61 time=1.02 ms
64 bytes from 10.1.5.2: icmp_seq=39 ttl=61 time=0.619 ms
64 bytes from 10.1.5.2: icmp_seq=40 ttl=61 time=0.636 ms
64 bytes from 10.1.5.2: icmp_seq=41 ttl=61 time=0.672 ms
64 bytes from 10.1.5.2: icmp_seq=42 ttl=61 time=0.634 ms
64 bytes from 10.1.5.2: icmp_seq=43 ttl=61 time=0.641 ms
64 bytes from 10.1.5.2: icmp_seq=44 ttl=61 time=0.564 ms
64 bytes from 10.1.5.2: icmp_seq=45 ttl=61 time=0.641 ms
64 bytes from 10.1.5.2: icmp_seq=46 ttl=61 time=0.614 ms
64 bytes from 10.1.5.2: icmp_seq=47 ttl=61 time=0.597 ms
64 bytes from 10.1.5.2: icmp_seq=48 ttl=62 time=0.402 ms     <----- The srl1-srl3 BGP route in being switch to, only two hops
64 bytes from 10.1.5.2: icmp_seq=49 ttl=62 time=0.647 ms
64 bytes from 10.1.5.2: icmp_seq=50 ttl=62 time=0.379 ms
64 bytes from 10.1.5.2: icmp_seq=51 ttl=62 time=0.407 ms
64 bytes from 10.1.5.2: icmp_seq=52 ttl=62 time=0.375 ms
64 bytes from 10.1.5.2: icmp_seq=53 ttl=62 time=0.390 ms
64 bytes from 10.1.5.2: icmp_seq=54 ttl=62 time=0.393 ms
64 bytes from 10.1.5.2: icmp_seq=55 ttl=62 time=0.446 ms
64 bytes from 10.1.5.2: icmp_seq=56 ttl=62 time=0.416 ms
64 bytes from 10.1.5.2: icmp_seq=57 ttl=62 time=0.458 ms
64 bytes from 10.1.5.2: icmp_seq=58 ttl=62 time=0.397 ms
64 bytes from 10.1.5.2: icmp_seq=59 ttl=62 time=0.364 ms
64 bytes from 10.1.5.2: icmp_seq=60 ttl=62 time=0.399 ms
64 bytes from 10.1.5.2: icmp_seq=61 ttl=62 time=0.381 ms
64 bytes from 10.1.5.2: icmp_seq=62 ttl=62 time=0.501 ms
64 bytes from 10.1.5.2: icmp_seq=63 ttl=62 time=0.373 ms
64 bytes from 10.1.5.2: icmp_seq=64 ttl=62 time=0.469 ms
64 bytes from 10.1.5.2: icmp_seq=65 ttl=62 time=0.478 ms
64 bytes from 10.1.5.2: icmp_seq=66 ttl=62 time=0.450 ms
64 bytes from 10.1.5.2: icmp_seq=67 ttl=62 time=0.371 ms
64 bytes from 10.1.5.2: icmp_seq=68 ttl=62 time=0.511 ms
64 bytes from 10.1.5.2: icmp_seq=69 ttl=62 time=0.470 ms
64 bytes from 10.1.5.2: icmp_seq=70 ttl=62 time=0.407 ms
```

## Explanation of BGP convergence

BGP convergence is the process by which all routers in a network reach a consistent, agreed-upon view of the best path to every destination after a topology change (link up, link down, new neighbor, withdrawn route).

### What BGP convergence actually did:

- When the link was broken (srl1:e1-3 admin-state disable):

1. The physical interface went down on srl1: immediately detectable at the hardware level
2. SR Linux withdrew the srl1 <-> srl3 BGP session
3. srl1 sent a BGP WITHDRAW message to srl2 for 10.1.5.0/24 (srl3's subnet)... except srl2 already had its own route to 10.1.5.0/24 via the direct srl2↔srl3 link (from Exercise 2)
4. srl2 re-advertised 10.1.5.0/24 back to srl1 as a BGP UPDATE (AS path: 65002 65003)
5. srl1 installed this as its new best path: 10.1.5.0/24 via 10.1.2.2 (srl2). Three hops in total: srl1 -> srl2 -> srl3 -> host3
6. This all happened before the next ping packet was sent: zero packet loss

- When the link was restored (srl1:e1-3 admin-state enable):

1. srl1:e1-3 came back up, srl1 -> srl3 BGP re-established
2. srl3 sent an UPDATE for 10.1.5.0/24 with AS path 65003 (length 1)
3. srl1 compared the two available paths:
   - Via srl2: AS path 65002 65003 (length 2)
   - Via srl3: AS path 65003 (length 1). The shorter wins
4. srl1 switched back to the direct path: TTL went back to 62 at seq=48

- Why no packets dropped on failover:

BGP convergence here was faster than the 1-second ping interval. Here are some possible explanations:

- The interface-down event is immediate: no waiting for a Hold Timer (30s) to expire
- SR Linux triggers BGP session teardown as soon as the physical link drops
- srl2 already had an alternate path pre-computed. It just needed to re-advertise it

This is the beauty of the self-healing property of dynamic routing: the network topology changed and the routers automatically found and agreed on a new forwarding path without any human intervention.
