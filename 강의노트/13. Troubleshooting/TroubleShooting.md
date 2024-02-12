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
