# Deliverables

## Before/after traceroute output

- Before:

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​
❯ docker exec clab-dynamic-routing-bgp-host2 traceroute -n -w 2 10.1.5.2
traceroute to 10.1.5.2 (10.1.5.2), 30 hops max, 60 byte packets
 1  10.1.4.1  1.204 ms  1.176 ms  1.170 ms
 2  10.1.2.1  0.424 ms  0.434 ms  0.435 ms
 3  10.1.3.2  0.579 ms  0.580 ms  0.577 ms
 4  10.1.5.2  0.462 ms  0.468 ms  0.488 ms
```

- After:

```sh
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/04-dynamic-routing-bgp main*​
❯ docker exec clab-dynamic-routing-bgp-host2 traceroute -n -w 2 10.1.5.2
traceroute to 10.1.5.2 (10.1.5.2), 30 hops max, 60 byte packets
 1  10.1.4.1  1.058 ms  1.019 ms  1.011 ms
 2  10.1.6.2  0.615 ms  0.609 ms  0.605 ms
 3  10.1.5.2  0.500 ms  0.508 ms  0.539 ms
```

## BGP route detail showing two paths with different AS path lengths

```sh
❯ docker exec -it clab-dynamic-routing-bgp-srl2 sr_cli -c "show network-instance default protocols bgp routes ipv4 prefix 10.1.5.0/24 detail"
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Show report for the BGP routes to network "10.1.5.0/24" network-instance  "default"
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Network: 10.1.5.0/24
Received Paths: 2
  Path 1: <Valid,>
    Route source    : neighbor 10.1.2.1
    Route Preference: MED is -, No LocalPref
    BGP next-hop    : 10.1.2.1
    Path            :  i [65001, 65003]
    Communities     : None
    RR Attributes   : No Originator-ID, Cluster-List is [ - ]
    Aggregation     : Not an aggregate route
    Unknown Attr    : None
    Invalid Reason  : None
    Tie Break Reason: as-path-length
  Path 2: <Best,Valid,Used,>
    Route source    : neighbor 10.1.6.2
    Route Preference: MED is -, No LocalPref
    BGP next-hop    : 10.1.6.2
    Path            :  i [65003]
    Communities     : None
    RR Attributes   : No Originator-ID, Cluster-List is [ - ]
    Aggregation     : Not an aggregate route
    Unknown Attr    : None
    Invalid Reason  : None
    Tie Break Reason: none

Path 2 was advertised to:
[ 10.1.2.1 ]
Route Preference: MED is -, No LocalPref
Path            :  i [65002, 65003]
Communities     : None
RR Attributes   : No Originator-ID, Cluster-List is [ - ]
Aggregation     : Not an aggregate route
Unknown Attr    : None
-------------------------------------------------------------------------------------------------------
```

## Explanation of which best path algorithm step decided the winner

The BGP best path algorithm walks through tie-breakers in order and stops at the first difference  
The direct path has a shorter AS path (1 hop vs 2 hops), so BGP selects it.
BGP doesn't pick the path with the lowest hop count in terms of router hops.  
It picks the path with the fewest AS hops. In this eBGP lab each router is its own AS, so the two happen to align, but the mechanism is AS Path Length, not physical hop count.
