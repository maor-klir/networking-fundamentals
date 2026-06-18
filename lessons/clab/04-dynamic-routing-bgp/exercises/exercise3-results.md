# Deliverables

## BGP neighbor detail showing sent-routes=0

```sh
--------------------------------------------------------------------------------------------------------------------------------------------
Peer : 10.1.3.1, remote AS: 65001, local AS: 65003, peer-type : ebgp
Type : static
Description : None
Group : ebgp-peers
Export policies : []
Import policies: ['import-all']
Under maintenance: False
Maintenance group:
--------------------------------------------------------------------------------------------------------------------------------------------
Admin-state is enable, session-state is established, up for 4d:1h:28m:11s
TCP connection is 10.1.3.2 [36379] -> 10.1.3.1 [179]
TCP-MD5 authentication is disabled
0 messages in input queue, 0 messages in output queue
--------------------------------------------------------------------------------------------------------------------------------------------
Last-state was openconfirm, last-event was recvOpen, 1 peer-flaps
Last received Notification was Error: None SubError: None
Failure detection: BFD is False, fast-failover is True
--------------------------------------------------------------------------------------------------------------------------------------------
Graceful Restart
   Admin State : disable
   Restarts by the peer : None
   Last restart : N/A
   Peer requested restart-time : None
   Stale routes time : None
--------------------------------------------------------------------------------------------------------------------------------------------
                  Timer                           Configured              Operational                           Next
=======================================================================================================================================
connect-retry                                         120                     120                                -
keepalive-interval                                    30                      30                                 -
hold-time                                             90                      90                                 -
minimum-advertisement-interval                         5                       5                                 -
prefix-limit-restart-timer                             0                       0                                 -
--------------------------------------------------------------------------------------------------------------------------------------------
Cap Sent:  ROUTE_REFRESH 4-OCTET_ASN MP_BGP GRACEFUL_RESTART
Cap Recv:  ROUTE_REFRESH 4-OCTET_ASN MP_BGP GRACEFUL_RESTART
--------------------------------------------------------------------------------------------------------------------------------------------
               Messages                                    Sent                                    Received                      Last
========================================================================================================================================
Non Updates                                                 523                                       523
Updates                                                      8                                         8                      2026-06-
                                                                                                                              18T18:10:4
                                                                                                                              9.400Z
Malformed updates                                            0                                         0
Route Refreshes                                              0                                         0
--------------------------------------------------------------------------------------------------------------------------------------------
Ipv4-unicast AFI/SAFI
    End of RIB                     : sent, received
    Received routes                : 5
    Rejected routes                : None
    Active routes                  : 2
    Advertised routes              : None
    Prefix-limit-received          : 4294967295 routes, warning at 90, prevent-teardown False
    Prefix-limit-accepted          : None
    Default originate              : disabled
    Advertise with IPv6 next-hops  : False
    Peer requested GR helper       : None
    Peer preserved forwarding state: None
--------------------------------------------------------------------------------------------------------------------------------------------
```

## Explanation of SR Linux default-deny export behavior

SR Linux's BGP implementation is built on a policy-first philosophy:Nothing is advertised to BGP peers unless you explicitly permit it.  
This is a security and operational safety feature: it prevents route leaks by design.
When a BGP session comes up in SR Linux, there is no implicit "advertise everything" behavior.  
Without an export-policy attached to a peer group, SR Linux's effective behavior is equivalent to a policy that rejects all routes, hence the term "default-deny".  
A BGP session being Established is not the same as routes being sent. The session handshake is separate from route advertisement.
