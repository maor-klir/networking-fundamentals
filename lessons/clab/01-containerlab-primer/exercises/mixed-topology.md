# Verification

```bash
~/repositories/github/maor-klir/kubecraft/lessons/clab/01-containerlab-primer main*​ 3m32s
❯ containerlab deploy -t exercises/mixed-topology.clab.yml
13:33:26 INFO Containerlab started version=0.76.0
13:33:26 INFO Parsing & checking topology file=mixed-topology.clab.yml
13:33:26 INFO Creating lab directory path=/home/maor/repositories/github/maor-klir/kubecraft/lessons/clab/01-containerlab-primer/exercises/clab-mixed-topology
13:33:26 INFO Creating container name=host1
13:33:26 INFO Creating container name=router
13:33:26 INFO Running postdeploy actions kind=srl node=router
13:33:26 INFO Created link: router:e1-1 ▪┄┄▪ host1:eth1
13:33:40 INFO Adding host entries path=/etc/hosts
13:33:40 INFO Adding SSH config for nodes path=/etc/ssh/ssh_config.d/clab-mixed-topology.conf
╭────────────────────────────┬───────────────────────────────┬─────────┬───────────────────╮
│            Name            │           Kind/Image          │  State  │   IPv4/6 Address  │
├────────────────────────────┼───────────────────────────────┼─────────┼───────────────────┤
│ clab-mixed-topology-host1  │ linux                         │ running │ 172.20.20.5       │
│                            │ alpine:3.20                   │         │ 3fff:172:20:20::5 │
├────────────────────────────┼───────────────────────────────┼─────────┼───────────────────┤
│ clab-mixed-topology-router │ srl                           │ running │ 172.20.20.6       │
│                            │ ghcr.io/nokia/srlinux:24.10.1 │         │ 3fff:172:20:20::6 │
╰────────────────────────────┴───────────────────────────────┴─────────┴───────────────────╯

~/repositories/github/maor-klir/kubecraft/lessons/clab/01-containerlab-primer main*​ 14s
❯ docker exec -it clab-mixed-topology-host1 sh
/ # ip addr show eth1
47: eth1@if48: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 9500 qdisc noqueue state UP
    link/ether aa:c1:ab:98:a8:1b brd ff:ff:ff:ff:ff:ff
    inet6 fe80::a8c1:abff:fe98:a81b/64 scope link
       valid_lft forever preferred_lft forever
/ # cat /etc/os-release
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.20.10
PRETTY_NAME="Alpine Linux v3.20"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://gitlab.alpinelinux.org/alpine/aports/-/issues"
```
