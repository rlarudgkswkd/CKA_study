# scheduling1

## Manual Scheduling
- 직접 파드를 스케쥴링 하고 싶을 경우

### How scheduling works
- nodeName : 부분이 초기 설정(ex. yaml등)에서 지정 되어 있지않고 create pod 할때 스케줄링 알고리즘이 자동으로 할당됨.

### No Scheduler

- 스케줄링이 없을 경우 pending 상태 유지됨.
- 없는 경우 업떻게 할당해줄까?
  - nodeName : 여기에 value 할당
- 이미 생성된 파드에 지정해주고 싶으면?
  - 기본적으로 생성시에만 할당 가능
  - 바인딩 객체를 생성해서 바인딩 API 게시 요청을 보내는것.

 
## Practice Test - Manual Scheduling
- 1번 문제 스케쥴링이 없는 것에 대한 로그는 어디에서 있는거지?
  - kubectl get pods --namespace kube-system
- ---
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  nodeName: node01 //위치 기억할것.
  containers:
  -  image: nginx
     name: nginx

- pending state 내용 : Node 가 NONE 으로 할당 되어 있어서
- kubectl get pods -o wide : 어느 노드에 할당되었는지도 확인 가능
---

## Labels and Selectors

