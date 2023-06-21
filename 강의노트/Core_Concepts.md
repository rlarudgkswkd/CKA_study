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

## Demo - Pods with YAML
- 파드를 yaml로 생성할거임.
- 여러 편집기로 생성 가능

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


