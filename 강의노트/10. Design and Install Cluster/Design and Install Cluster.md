# Design and Install Cluster

## Design a k8s cluster

## choosing kubernetes infra

## Configure High Availability
- 만약 master노드가 죽으면?
  - application은 계속 살아있으나 새로 만들거나 관리가 불가능할것
  - 그래서 HA에서는 multi master node 를 구성함.
- 마스터 노드는 hosting 역할함.
- 다른거에 api server, etcd, controller manager, scheduler 를 동일하게 구성함.
- 두개에 도달하기 위해 HA proxy 등을 고려할수 있음.
- 두개는 병렬적인 처리가 아니라 leader election 처리를 함. 한번에 하나만 active 되서 동시성 보장.
- External ETCD Topology
  - 별도 서버로 빼버리는 거임.
![image](https://github.com/rlarudgkswkd/CKA_study/assets/48428850/361b83d6-f1c8-45fc-93b0-9ed712e87f3b)

## ETCD In HA
- what is etcd
  - 분산 처리된 key value store
- 3중 되어있을경우 어떻게 될까?
  - write일 경우 leader election 전략 취함
- 리더는 어케 뽑음?
  - Leader Election -RAFT : 타임 돌때마다 리더가 바뀜
- quorum = n/2 + 1
  - quorum of 3 = 3/2 + 1 = 2.5~ = 2
  - quorum of 5 = 5/2 + 1 = 3.5~ = 3
