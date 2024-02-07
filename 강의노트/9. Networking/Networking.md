
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
## Prerequisite - DNS
- ping을 ip로만 보내다가 db라는 이름으로 보내려면 못참을거임
- 그럴때 cat /etc/hosts 를 볼것.
  - 192.168.1.11 db를 추가하고 저장하면 됨.
  - ping db 되는거 확인
- 그러나 이렇게 할때 여러개의 서버가 생겨버리면 관리하기가 어려워짐. 이거를 한개의 서버로 관리하자는게 DNS 서버임.

### DNS 서버

### Domain Names

### nslookup
- ping으로 DNS 체크하는것보다 나음
- dig

## Prerequisite - Network Namespaces

### Create Network NS
- ip netns add red
- ip netns add blue
- ip netns

### EXEC IN NETWORK NS
- ip link
  - ip netns exec red ip link
  - ip -n red link
- arp
  - ip netns exec red arp
- route
  - ip netns exec red route
 
### create the cable between red and blue
- ip link add veth-red type veth peer name veth-blue
- ip link set veth-red netns red
- ip link set veth-blue netns blue
- ip -n red addr add 192.168.15.1 dev veth-red
- ip -n blue addr add 192.168.15.2 dev veth-blue
- ip -n red link set 192.168.15.1 veth-red up
- ip -n blue link set 192.168.15.1 veth-blue up
- 등등

### LINUX BRDIGE
- switch

## Prerequisite - Docker Networking
### None
### HOST
### Bridge
- 
