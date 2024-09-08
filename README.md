# docker-networking
Docker Networking Stuff

Taken from NetworkChuck - Docker networking is CRAZY!!
https://youtu.be/bKFMS5C4CG0?si=Df6J9ZRZk4pHK1Te

## Commands

#### Docker network
```
$ docker network ls

NETWORK ID     NAME        DRIVER    SCOPE
4871c887007d   bridge      bridge    local
bb5c4a16fe34   host        host      local
2aff5e84d0f7   none        null      local
```

#### Docker run
```
$ docker run -itd --rm --name thor busybox
$ docker run -itd --rm --name mjolnir busybox
$ docker run -itd --rm --name stormbreaker nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
Status: Downloaded newer image for nginx:latest
```

#### Docker ps
```
$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS     NAMES
2dbd39703a70   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   80/tcp    stormbreaker
199ac648b515   busybox   "sh"                     2 minutes ago        Up 2 minutes                  mjolnir
03c77f5b00cb   busybox   "sh"                     3 minutes ago        Up 3 minutes                  thor
```

Current setup
![](./Default%20Bridge%202.png)

#### ip add show
<details>
<summary>Click to expand</summary>

#### show all ip's
```
$ ip add show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:e1:9d:bd brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 62696sec preferred_lft 62696sec
    inet6 fe80::a50b:b2ed:61dc:365b/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:e2:3c:73 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.14/24 brd 192.168.1.255 scope global dynamic noprefixroute enp0s8
       valid_lft 62696sec preferred_lft 62696sec
    inet6 2002:219:8bf9:0:5c50:de4:6061:e220/64 scope global temporary dynamic 
       valid_lft 2264sec preferred_lft 1664sec
    inet6 2002:219:8bf9:0:a00:27ff:fee2:3c73/64 scope global dynamic mngtmpaddr 
       valid_lft 2264sec preferred_lft 1664sec
    inet6 fe80::a00:27ff:fee2:3c73/64 scope link 
       valid_lft forever preferred_lft forever
4: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:8e:31:3c:4c brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:8eff:fe31:3c4c/64 scope link 
       valid_lft forever preferred_lft forever
7: veth82ba4be@if6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether 1e:8b:84:5b:f5:cd brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::1c8b:84ff:fe5b:f5cd/64 scope link 
       valid_lft forever preferred_lft forever
9: veth4572fc3@if8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether c2:8f:24:b4:50:f6 brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet6 fe80::c08f:24ff:feb4:50f6/64 scope link 
       valid_lft forever preferred_lft forever
11: veth2ab40f2@if10: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether f2:b7:7a:7e:f3:6e brd ff:ff:ff:ff:ff:ff link-netnsid 2
    inet6 fe80::f0b7:7aff:fe7e:f36e/64 scope link 
       valid_lft forever preferred_lft forever
```
</details>

#### bridge link
```
$ bridge link
7: veth82ba4be@if6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 master docker0 state forwarding priority 32 cost 2 
9: veth4572fc3@if8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 master docker0 state forwarding priority 32 cost 2 
11: veth2ab40f2@if10: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 master docker0 state forwarding priority 32 cost 2
```

#### Docker inspect
<details>
<summary>Click to expand</summary>

#### Docker inspect bridge
```
$ docker inspect bridge
[
    {
        "Name": "bridge",
        "Id": "4871c887007dd12af9ef5b1e0fa1a678d8c0585451c7a9072d46ed33af86afa9",
        "Created": "2024-09-08T10:32:01.310467028+01:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "03c77f5b00cbce6f1af718073bffdc0ca407f93e260c43c4131ca2fe05408b65": {
                "Name": "thor",
                "EndpointID": "a90c0ed9748e083ed6867a1513ca6c248a60bb4292f67b916e19dcc3c8400d8f",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            },
            "199ac648b515d0f44512003ec88be63ce910d0173726d383488fc64c5316d084": {
                "Name": "mjolnir",
                "EndpointID": "86e67549e9cfbc679652d84d707f2394e12d51245bc364e6ac95336424d5bef2",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            },
            "2dbd39703a70e1632b3c1027bcefcfc7ab3fc6edbf171838c2795631daacd066": {
                "Name": "stormbreaker",
                "EndpointID": "15e8d1465034285d7c1d50d784d6f1aabe407a18cc31654b7047f3bd1b3d5911",
                "MacAddress": "02:42:ac:11:00:04",
                "IPv4Address": "172.17.0.4/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```
</details>