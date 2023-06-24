# Core Concepts


## k8s Architecture
- Worker node : 액션 단위
- Master node : 컨트롤
- Kube API server
- ETCD Cluster
- Kube-scheduler
- Controller-Manager
- kube-proxy

---

## ETCD
- 버전이 2개 지원
    - 꼭 버전 확인하고 하셈
    - version 2,3 두개 존재
- k8s 관련 정보를 저장함

---

## 클러스터, 노드, 파드, 컨테이너
- 링크 : https://eng-sohee.tistory.com/m/129

---

## Kube-scheduler
- 스케쥴링에 대해 구성하는 것 뿐이지 실제로 pod를 옮기고 적재하고 이런 action을 하지는 않음. action은 kubelet

---

## Recap - Pods
- 클러스터 -> Node -> Pod 단위로 진행
- k8s 클러스터 -> Single Node -> Single Pod 환경
![image](https://github.com/rlarudgkswkd/CKA_study/assets/48428850/fd2e74ca-fba0-43b1-b2b3-c25d2dc9d875)
- 도커 컨테이너
    - Single Node 에서 여러개의 컨테이너를 실행 하게 됌.
    - 그리고 컨테이너에 해당하는 helper 컨테이너를 실행하게됨 1:1 매칭으로
    - 그리고 매핑을 관리함. 이런 환경에서는 사용자가 알아서 해야하는데
    - Helper 와 App 컨테이너를 pod 단위로 묶어서 관리하면 편해짐. 장기적으로 좋음
- Pod Deployment
    - kubectl run nginx : 이미지는 어디서 얻음? -> Docker Hub repo 에서 가져옴
    - 상태 확인 : kubectl get pods -> 여기 까지는 외부 사용자가 접근할수 없는 상황. (추후 네트워킹 까지 진행)

---

## Pods with YAML
- YAML in Kubernetes : yaml을 복제본, 배포 등 생성을 위한 입력으로 사용
- 4개의 TOP 레벨 : apiVersion, kind, metadata, spec -> 이거 필수
- apiVersion : 버전
- kind : pod
- metadata : 이름이나 라벨 같은거
 - name  :
 - labels : 라벨링 해서 필터링을 하기 위함 key value 값
    - app : myapp
    - type : front-end
- spec
 - containers
 - etc 
![image](https://github.com/rlarudgkswkd/CKA_study/assets/48428850/80434e58-22d0-4c09-951f-e196407b938b)

---

## Demo - Pods with YAML
- 파드를 yaml로 생성할거임.
- 여러 편집기로 생성 가능

---

## Practice Test - Pods
- 링크 : https://kodekloud.com/topic/practice-test-pods-2/
```
kubectl get pods
kubectl get pods -o wide
kubectl describe pods/webapp
kubectl run nginx --image=nginx

NAME            READY   STATUS             RESTARTS   AGE
nginx           1/1     Running            0          15m
newpods-xxcbc   1/1     Running            0          15m
newpods-m5zvc   1/1     Running            0          15m
newpods-kwgwr   1/1     Running            0          15m
webapp          1/2     ImagePullBackOff   0          10m
-> 여기서 1/1 , 1/2 은 running 중인 컨테이너/전체 컨테이너 수 임

kubectl delete pods/webapp //webapp이라는 이름의 pod를 삭제함
kubectl delete pod webapp // 이렇게도 가능
```

- pod-definition.yaml로 pod 생성
  - 참고링크 : https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/
```
apiVersion: v1
kind: Pod
metadata:
  name: redis
spec:
  containers:
  - name: redis
    image: redis123
    volumeMounts:
    - name: redis-storage
      mountPath: /data/redis
  volumes:
  - name: redis-storage
    emptyDir: {}
```

- kubectl apply -f pod-definiton.yaml 로 실행
- 다른 방식도 있음
```
    kubectl run redis --image=redis123 --dry-run -o yaml  // --dry-run 이 deprecated 됐다함.
    kubectl run redis --image=redis123 --dry-run=client -o yaml //정상 결과 출력
    kubectl run redis --image=redis123 --dry-run=client -o yaml > redis.yaml //파일로 만들기
    cat redis.yaml
    kubectl create -f redis.yaml
    kubectl get pods
```
- 추가
  
```
    cat redis.yaml
    vi redis.yaml -> redis123을 redis 로 변경 후 저장
    kubectl apply -f redis.yaml
```

---

## Replication Controller & Replica Set
- 왜 필요한지?
  - 응용프로그램을 실행하는 싱글 노드와 파드 있을때 파드의 문제가 생김 그러면 유저의 사용에 지장이 없게 그대로 실행되게 해야함. 복제품을 생성 시켜주는 것이 Replication Controller
### 필요이유1 : High Availability
- 항상 실행되게 보장해주는 것. 이것이 고가용성 보장 High Availability
### 필요이유2 : Load Balancing & Scaling
- 여러개의 파드를 생성해서 로드가 공유되게 하려함.
- 단일포드가 사용자 집합을 담당하다가 많아지면 여러개의 파드가 부하를 대응하게 만들어줌.
### Replication Controller vs Replica Set
- 두개는 다르나 앞서 언급한 두개의 동작을 다 할수있음
- 요즘은 Replica Set으로 사용
- rc-definition.yaml
  ```
  apiVersion : v1
  kind :
  metadata:
      name: myapp-rc
      labels:
          app: myapp
          type: front-end
  spec :
      template: {{pod-template 내용}} //기존 pod-definition.yaml의 metadata 애들을 자식으로 넣기
  //위에 metadata는 replication controller를 위한거고 아래의 template 아래 metadata는 pod를 위한것.
      replicas: 3          
  ```
  - kubectl create -f rc-definition.yaml // -f는 파일 파라미터
  - kubectl get replicationcontroller
  - kubectl get pods

### Replica Set
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
    name: myapp-replicaset
    labels:
        app: myapp
        type: front-end
spec:
    template: {{pod-template 내용}}
replicas: 3
selector: //Replcation Controller와의 큰 차이, 이게 필요함, 어떤 파드가 아래있는지를 정의해야함.
        //<U>!!여기 개념 잘 이해안됌.</U>
    matchLabels:
        type: front-end 
        
```
- kubectl create -f replicaset-definition.yml
- kubectl get replicaset
- kubectl get pods

### Labels and Selectors
- 왜 k8s는 label이랑 selector를 쓸까?
- 수십개의 파드들 중에 관리하기 위해서 selector를 사용 그래서 matchLabels 사용
- 구조 : 템플릿, 복제본, 선택기

### Scale
- 3개로 정의 했는데 만약 6개로 넘기고 싶으면?
- definition 파일을 수정 : 6개로
- kubectl replace -f replicaset-definiton.yml
- kubectl scale --replicas=6 -f replicaset-definition.yml //방식1
- kubectl scale --replicas=6 replicaset myapp-replicaset //방식2

### Commands
- kubectl create -f replicatset-definition.yml
- kubectl get replicaset
- kubectl delete replicaset myapp-replicaset
- kubectl replace -f replicaset-definition.yml
- kubectl scale --replicas=6 -f replicaset-definition.yml 

---


