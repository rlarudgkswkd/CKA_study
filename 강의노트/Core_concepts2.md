# Core_concepts2

## Deployments
- 여러개의 웹서버에 배포 되어야 하는 상황
- Docker 를 통해 할수 있는 상황이지만 하나씩 하나씩 해야함.
- 한개씩 하던중에 한곳에서 에러가 나서 다시 돌려야하면? 또 하나씩해야함.
- k8s deployment로 다 해결가능
  - 각 컨테이너는 캡슐 형태로 포장됨 Pod
  - 그리고 Pod는 Replica Set으로 묶임
  - 그리고 Replica set은 Deployment로 묶이는것. (더 높은 계층)

### Definition
```
apiVersion: apps/v1
kind: Deployment
metadata
.
.
.
.
```
![image](https://github.com/rlarudgkswkd/CKA_study/assets/48428850/22fa122c-508f-45fd-8e41-806c59d4cbb0)

- kubectl create -f deployment-definition.yml
- kubectl get deployments -> 배포가 자동으로 Replicaset 을 형성

### Commands
- kubectl get all //전부다 볼수있는 명령어

----

## Tips!!
- yaml 파일로 하려고 할때 어려움이 있음
- 그냥 kubectl run 하고 {{이미지}}나 {{이름}} 이렇게 하면 도움을 줌.
```
//Create an NGINX Pod

kubectl run nginx --image=nginx

//Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

kubectl run nginx --image=nginx --dry-run=client -o yaml

//Create a deployment

kubectl create deployment --image=nginx nginx

//Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

kubectl create deployment --image=nginx nginx --dry-run=client -o yaml

//Generate Deployment YAML file (-o yaml). Don’t create it(–dry-run) and save it to a file.

kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml

//Make necessary changes to the file (for example, adding more replicas) and then create the deployment.

kubectl create -f nginx-deployment.yaml

OR

//In k8s version 1.19+, we can specify the --replicas option to create a deployment with 4 replicas.

kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```

---

## Practical Tests
- vim  deployment.yaml

---

## Practice Tests Solution
- kubectl create deployment --help : 해당 명령어로 도움 받을수 있음
- 특정 이미지로 Deployment 생성 : yaml 파일을 수정해서 해도되지만 다른 명령어로도 가능
  - kubectl create deployment httpd-frontend --image=httpd:2.4-alpine --replicas=3

----

## Services
- 서비스는 애플리케이션 안밖의 각종 통신과 연결을 도와줌.
- 프론트, 백엔드, DB 그룹이 있다고 함. 각 사이에연결을 담당함.
- 192.168.1.2 node 가 있고 안에 네트워크는 10.244.0.0 pod는 10.244.0.2
  - 외부 접속을 위해서 무엇인가 필요
  - 여기에서 service가 필요 nodeport

### Service Types
- NodePort : 노드 내부에 포트로 접속하기 위한것.
- ClusterIp : 가상 ip를 만들어서 서비스 간의 통신이 가능하게 해줌.
- Load Balancer : 부하 분산기를 프로비전

### NodePort
- Port
- TargetPort
- NodePort range : 30000 - 32767
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: myapp-service
  spec:
    type: NodePort
    ports:
      - targetPort: 80
        port: 80 //port 만 필수요소이고 나머지는 알아서 지정됌.
        nodeport: 30008
  ```
  - 여기에는 어떤pod랑 연결하는지가 없음 -> 여기서 selector 사용
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: myapp-service
  spec:
    type: NodePort
    ports:
      - targetPort: 80
        port: 80 //port 만 필수요소이고 나머지는 알아서 지정됌.
        nodeport: 30008
    selector:
      app: myapp
      type: front-end
  ```
  - kubectl create -f service-efinition.yaml
  - kubectl get services
  - curl http://192.168.1.2:30008
