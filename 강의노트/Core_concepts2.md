# Core_concepts2 from Deployment

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
```
![image](https://github.com/rlarudgkswkd/CKA_study/assets/48428850/22fa122c-508f-45fd-8e41-806c59d4cbb0)

- kubectl create -f deployment-definition.yml
- kubectl get deployments -> 배포가 자동으로 Replicaset 을 형성

### Commands
- kubectl get all //전부다 볼수있는 명령어

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

