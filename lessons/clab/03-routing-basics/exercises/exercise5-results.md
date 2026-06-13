# Deliverables

## Traceroute output showing the routing loop pattern

```sh
❯ docker exec clab-routing-basics-host1 traceroute -n -w 2 10.1.5.2
traceroute to 10.1.5.2 (10.1.5.2), 30 hops max, 60 byte packets
 1  10.1.1.1  0.817 ms  0.787 ms  0.779 ms
 2  10.1.2.2  0.504 ms  0.506 ms  0.503 ms
 3  10.1.2.1  0.732 ms  0.724 ms  0.718 ms
 4  10.1.2.2  1.297 ms  1.294 ms  1.288 ms
 5  * * *
 6  10.1.2.2  1.256 ms  0.844 ms  0.826 ms
 7  * * *
 8  10.1.2.2  0.562 ms * *
 9  * * *
10  * * *
11  * * *
12  10.1.2.2  1.855 ms  1.856 ms  1.859 ms
13  * * *
14  10.1.2.2  1.816 ms  1.814 ms  1.813 ms
15  * * *
16  10.1.2.2  1.774 ms  1.769 ms  1.766 ms
17  * * *
18  10.1.2.2  0.875 ms * *
19  * * *
20  * 10.1.2.2  1.896 ms  1.878 ms
21  * * *
22  10.1.2.2  1.855 ms  1.857 ms  1.858 ms
23  * * *
24  10.1.2.2  1.838 ms  1.839 ms  1.839 ms
25  * * *
26  10.1.2.2  1.875 ms  1.872 ms *
27  * * *
28  * * *
29  * * *
30  10.1.2.2  1.893 ms  1.895 ms  1.890 ms
```

## Explanation of TTL and how it prevents infinite loops

Every IP packet starts with a TTL (Time To Live) value, typically 64.  
Each router that forwards a packet decrements TTL by 1. When TTL hits 0, the router drops the packet and sends back an ICMP "Time Exceeded" message. This is the safety mechanism that prevents packets from bouncing forever in a loop.
