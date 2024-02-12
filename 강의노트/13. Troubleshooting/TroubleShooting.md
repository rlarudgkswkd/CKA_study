# TroubleShooting

## Application Failure
- 유용한 명령어
```
k config set-context --current --namespace=alpha
curl http://localhost:30081
k replace --force -f /tmp/kubectl-edit-3195977508.yaml
```

## Controlplane Failure
- k get nodes
- k get pods
- k get pods -n kube-system
- service kube-apiserver status
- service kube-controller-manager status
- service kube-scheduler status
- service kubelet status
- service kube-proxy status
- k logs kube-apiserver-master -n kube-system
- sudo journalctl -u kube-apiserver

## Controlplane Failure Practice test
- cd /etc/kubernetes/manifests 하위 경로 파일 수정
- replicaset 관련 역할은 kube-controller가 진행함.

## Worker Node Failure
- kubectl get nodes
- k describe node worker-1
- top
- df -h
- service kubelet status
- sudo journalctl -u kubelet
- openssl x509 -n /var/lib/kublet/worker-1.crt -text
- 각 노드별로 있는 cat /var/lib/kubelet/config.yaml

## Worker Node Failure - Practice Test
### Quiz 1
- ssh node01
- service kubelet status
- service kubelet start
### Quiz 2
- kubectl describe node node01
- ssh node01
- service kubelet status
- service kubelet start
- service kubelet status
- journalctl -u kubelet -> 제일 나중로그보기
- find unable to load client CA file
- cat /etc/kubernetes/kubelet.conf
- ls /var/lib/kubelet
- cat /var/lib/kubelet/config.yaml
  - find wrong path and edit it
- service kubelet restart
### Quiz 3
- ssh node01
- service kubelet status
- journalctl -u kubelet -> 제일 나중 로그
  - unable to register node
  - wrong port
- cat /etc/kubernetes/kubelet.conf
- vi /etc/kubernetes/kubelet.conf -> 포트 수정
- service kubelet restart
- exit k get node

## Network Troubleshooting
```
- weave net
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml

- flannel
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/2140ac876ef134e0ed5af15c65e414cf26827915/Documentation/kube-flannel.yml

- Calico
curl https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml -O

Apply the manifest using the following command.

kubectl apply -f calico.yaml

proxy . /etc/resolv.conf



```

- https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/
- https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/
