# Core Concepts3

## Imperative vs Declarativ
- IaaC 에는 두개의 접근법이 있음
- Imperative : 스탭별로 구성
- Declarative : 목표를 먼저 정해줌. 시스템이 가는 경로를 알아서 찾아줌. what이 아닌 HOW TO DO임

### Infrastructure as Code
- Imperative : 서버 구성 요소를 다 작성함
  - kubectl run --image=nginx nginx
  - kubectl create deployment --image=nginx nginx
  - kubectl expose deployment nginx --port 80
  - kubectl edit deployment nginx
  - kubectl scale deployment nginx --replicas=5
  - kubectl set image deployment nginx nginx=nginx:1.18
  - kubectl create -f nginx.yaml
  - kubectl replace -f nginx.yaml
  - kubectl delete -f nginx.yaml
- Declarative : yaml 파일에 다 정의함.
  - kubectl apply -f nginx.yaml
