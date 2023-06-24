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
![Uploading image.png…]()
- kubectl create -f deployment-definition.yml
- kubectl get deployments -> 배포가 자동으로 Replicaset 을 형성

### Commands
- kubectl get all //전부다 볼수있는 명령어
