# Core Concepts3

## Imperative vs Declarative
- IaaC 에는 두개의 접근법이 있음
- Imperative : 스탭별로 구성
- Declarative : 목표를 먼저 정해줌. 시스템이 가는 경로를 알아서 찾아줌. what이 아닌 HOW TO DO임

### IaaC - Imperative
- Imperative : 서버 구성 요소를 다 작성함
  - Create Objects
    - kubectl run --image=nginx nginx
    - kubectl create deployment --image=nginx nginx
    - kubectl expose deployment nginx --port 80
    - kubectl create -f nginx.yaml // 이런 방식이 추후 유지보수에 도움됨. 어떤 설정했는지 알수있으니
  - Update Objects
    - kubectl edit deployment nginx
    - kubectl scale deployment nginx --replicas=5
    - kubectl set image deployment nginx nginx=nginx:1.18
    - kubectl replace -f nginx.yaml // 마찬가지로 휘발성이 아니기에 유지 보수 가능
    - kubectl delete -f nginx.yaml

### IaaC - Declarative
- Declarative : yaml 파일에 다 정의함. apply 사용
  - Create Objects
    - kubectl apply -f nginx.yaml
    - kubectl apply -f /path/to/config-files
  - Update Objects
    - kubectl apply -f nginx.yaml

### Certification Tips
```
//POD
//Create an NGINX Pod
kubectl run nginx --image=nginx

//Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
kubectl run nginx --image=nginx --dry-run=client -o yaml

//Deployment
//Create a deployment
kubectl create deployment --image=nginx nginx

//Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml

//Generate Deployment with 4 Replicas
kubectl create deployment nginx --image=nginx --replicas=4


//You can also scale a deployment using the kubectl scale command.
kubectl scale deployment nginx --replicas=4

//Another way to do this is to save the YAML definition to a file and modify
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml

//You can then update the YAML file with the replicas or any other field before creating the deployment.

//Service
//Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml

(This will automatically use the pod's labels as selectors)

Or

kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml (This will not use the pods labels as selectors, instead it will assume selectors as app=redis. You cannot pass in selectors as an option. So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)


//Create a Service named nginx of type NodePort to //expose pod nginx's port 80 on port 30080 on the nodes:
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml

(This will automatically use the pod's labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or

kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```

---
