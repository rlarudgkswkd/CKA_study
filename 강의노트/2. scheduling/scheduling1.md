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
- 그룹으로 묵는 표준 방법임. ex. 종별로 묶는것
- 필요에 따라 물건을 분류하고 묶는 방법이 필요한데 그때 Labels 사용하는 것.

### Labels & Selectors in K8s
- k8s에는 object가 다 달라짐.
- 수백 수천개가 되면 그룹별로 봐야함. ex. app별, 기능 별 등

### Labels
```
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
  labels:
    app: App1
    function: Front-end

spec:
  containers:
  - name: simple-webapp
    image: simple-webapp
    ports:
      - containerPort: 8080
```

### Select
```
kubectl get pods --selector app=App1
```

### Replicaset
- replicaset 의 labels와 복제 대상의 labels를 혼동하지 말것.

### Annotations
- 주석 같은 느낌으로 사용 가능

---

## Practice Test - Labels and Selectors

- k get pods --selector env=dev
- !! 3번 문제 확인 필요 : 내가 낸 답
  - k get all --selector env=prod
- 4번 문제 : k get pod --selector env=prod,bu=finance,tier=frontend
- 결과값 세기 : k get pods --selector env=dev | wc -l

---

## Taints and Tolerations

- 
