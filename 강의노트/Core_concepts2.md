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
- Pod 가  여러개일 경우?
  - selector와 label을 같은 이름으로 지정하기.
- 여러개의 노드일 경우
  - 서비스로 다 묶고 같은 포트로 ip만 다르게 통신가능함.

---

##  Services Cluster IP
- 풀 스택 웹 호스팅은 각각 다른 포드가 있음
	- 프론트, 백엔드, redis, 데이터 베이스
	- 각각 내용들은 어떻게 통신할까? internal 소통으로만 처리 할수 없음.
	- 누구에게 ip 를 줄지 이런것들을 어떻게 결정할까?
	- 개별 인터페이스로 묶어서 처리가 가능함.	
```
apiVersion: v1
kind: Service
metadata:
	name: back-end
spec:
	type: ClusterIP
	ports:
		- targetPort: 80
			port: 80
	selector:
		app: myapp
		type: back-end
```
- kubectl create -f service-definition.yaml
- kubectl get services
  
---

##  Services - Loadbalancer
- 외부와 직면한 응용프로그램이 워커 노드를 사용하도록 도움.
- http://example-vote.com 등의 도메인으로 한번에 접속하게하려면? 
	- Load Balancer로 설정함.
```
apiVersion: v1
kind: Service
metadata:
	name: myapp-service
spec:
	type: LoadBalancer //이거 내용만 추가하면됨.
	ports:
		- targetPort: 80
			port: 80
			nodePort: 30008
```

---

## Practice Test - Services

- 서비스를 통해 레이블과 selector로 진입점을 넣어주는것.
- label을 만약 다르게 했을때는 어떻게됌.? 접속 안됌.

---

## Namespaces

- 두명의 같은 이름을 가진 사람이 있음. 성이 각각 스미스와 윌리엄
- 스미스씨의 집에서는 다른 사람들이 마크라고 부르지만 윌러엄을 부르고 싶으면 마크 윌리엄스라고 부름.
- 집들은 k8s의 네임스페이스라고 할수있음.
- 처음 우리가 할때 부터도 모든 리소스 생성은 default 네임스페이스에서 진행했음
- 내부 목적을 위해 kube-system 이라는것 존재
- kube-public : 모든 사용자가 사용할 리소스가 존재
- 작은 단위의 규모로 응용하면 default에서 사용가능하지만 기업이나 배포를 위해서는 네임스페이스를 분리해야함. ex. Dev와 Prod
- 그리고 리소스 limits도 가능함.
- 각각의 리소스들이 소통하기 위해 DNS 필요
  - mysql.connect("db-service.dev.svc.cluster.local")

```
kubectl get pods
kubectl get pods --namespace=kube-system //특정 네임스페이스에 접근하고 싶을때 사용
```

### pod-definition.yml 파일로 별도 네임스페이스에 생성
```
kubectl create -f pod-definition.yml
kubectl create -f pod-definition.yml --namespace=dev
```

### Create Namespace
- namespace-dev.yml
```
apiVersion: v1
kind: Namespace
metadata:
	name: dev
```
- kubectl create -f namespace-dev.yml
- kubectl create namespace dev

### Switch
- kubectl config set-context $(kubectl config current-context) --namespace=dev : 현재 네임 스페이스 설정하기
- kubectl get pods

### Resource Quota
- spec 제한 하는것.
- Compute-quota.yml
```
apiVersion: v1
kind: ResourceQuota
metadata:
	name: compute-quota
	namespace: dev
spec:
	hard:
		pods: "10"
		requests.cpu: "4"
		requests.memory: 5Gi
		limits.cpu: "10"
		limits.memory: 10Gi
```
