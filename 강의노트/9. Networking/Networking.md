
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

## PRACTICE TEST – EXPLORE ENVIRONMENT
```
ip a
ip link
ip route show default
ip address show eth0
ip address show type bridge
netstat --help
netstat -l
netstat | grep -i scheduler
netstat -npl | grep -i scheduler
netstat -npa | grep -i etcd | grep -i 2379 | wc -l
netstat -npa | grep -i etcd | grep -i 2380 | wc -l
```
## Pod Networking

### setting network
- ip link add v-net-o type bridge
- ip link set dev v-net-o up
- ip addr add 10.244.1.1/24 dev v-net-o

### Script
```Script.sh
# Create veth pair
ip link add ....

# Attach veth pair
ip link set ....
ip link set ....

# Assign IP Address
ip -n <namespace> addr add ....
ip -n <namespace> route add ....

# Bring up Interface
ip -n <namespace> link set ....
```

### 다른 노드에 있는 pod와 연결
- A파드에서 B로 ping
  - ping 10.244.2.2 -> network is unreachable
- 속한 노드1로
  - ip route add 10.244.2.2 via 192.168.1.12 (B가 속한 노드2의 ip)
  - ping 10.244.2.2 이제 됨

## CNI in kubernetes
- CNI는 container runtime에 책임이 있음

### Configuring CNI
- kubelet에서 돔
- ls /opt/cni/bin
- ls /etc/cni/net.d
  - cat /etc/cni/net.d/10-bridge.conf
 
## Practice Test
- ps -aux | grep kubelet | grep container-runtime-endpoint
- ls /opt/cni/bin
- ls /etc/cni/net.d/

## Practice Test - Service Network
- cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep service-cluster-ip-range

## CoreDNS in K8s
### Kube-dns
- cat /etc/resolv.conf -> DNS 주소가 각 파드별로 등록됨. /etc/host 대체 느낌.
### Core-DNS
- 2개로 배포됨.
- cat /etc/coredns/Corefile
- kubectl get configmap -n kube-system
- kubectl get service -n kube-system -> kube-dns
  ```
  ## 모든 주소가 같은 곳을 가리킴
  curl http://web-service
  curl http://web-service.default
  curl http://web-service.default.svc
  curl http://web-service.default.svc.cluster.local
  ```

## CoreDNS Practice Test
- 


## Ingress
- 여러가지 ingress-controller 존재
- nginx 관련해서 볼거임
- ingress 설정을 위해 3가지 필요
  - ingress-controller : nginx-ingress-controller deploy
  - configuration file : configmap
    - error-log-path
    - keep alive
    - ssl protocols
  - Service : nginx-ingress , NodePort (외부 통신용)
  - ServiceAccount : nginx-ingress-serviceaccount
 
### Ingress Resource
- ingress-controller 관련된 규칙을 관리함.
  - url 이나 route 등
- ingress-wear.yaml
```
apiVersion: extensions/v1betal
kind: Ingress
metadata:
  name: ingress-wear
spec:
  backend:
    serviceName: wear-service
    servicePort: 80
```
- kubectl get ingress
- 404 page도 설정 가능
![image](https://github.com/rlarudgkswkd/CKA_study/assets/48428850/7133b237-9c8f-43bf-bf71-88e153f3e15d)

- 두가지 종류 존재
  1. rule 1개 path 2개
  2. rule 2개

## Ingress Practice Test 1
- 11번

## Ingress Practice Test 2
- 7번
  ```
  apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx-ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /
  creationTimestamp: "2024-02-10T13:16:04Z"
  generation: 1
  name: ingress-wear-watch
  namespace: app-space
  resourceVersion: "2387"
  uid: c2a516b5-aa0e-4f1a-83ad-ff2f82303bf4
spec:
  rules:
  - http:
      paths:
      - backend:
          service:
            name: wear-service
            port:
              number: 8080
        path: /wear
        pathType: Exact
      - backend:
          service:
            name: video-service
            port:
              number: 8080
        path: /watch
        pathType: Exact
status:
  loadBalancer:
    ingress:
    - ip: 10.98.161.59
  ```
- 이게 추가되서 해결됨
  - nginx-ingress.kubernetes.io/ssl-redirect: "false"
