# Core Concepts


## k8s Architecture
- Worker node : 액션 단위
- Master node : 컨트롤
- Kube API server
- ETCD Cluster
- Kube-scheduler
- Controller-Manager
- kube-proxy


## ETCD
- 버전이 2개 지원
    - 꼭 버전 확인하고 하셈
    - version 2,3 두개 존재
- k8s 관련 정보를 저장함

## 클러스터, 노드, 파드, 컨테이너
- 링크 : https://eng-sohee.tistory.com/m/129

## Kube-scheduler
- 스케쥴링에 대해 구성하는 것 뿐이지 실제로 pod를 옮기고 적재하고 이런 action을 하지는 않음. action은 kubelet
