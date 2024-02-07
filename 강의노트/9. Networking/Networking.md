
# Networking

## Prerequisite - Switching Routing
```
ip link
ip addr
ip addr add 192.168.1.10/24 dev eth0
```

- 이거 영구적으로 적용하려면 /etc/network/interfaces file에 저장해야됨

```
ip route
ip route add 192.168.1.0/24 via 192.168.2.1
cat /proc/sys/net/ipv4/ip_forward
```
