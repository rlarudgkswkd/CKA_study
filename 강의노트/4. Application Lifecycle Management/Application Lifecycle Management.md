# Application Lifecycle Management

## Commands and Arguments

## Commands and Arguments Pratice Tests
```
kubectl run webapp-green --image=kodekloud/webapp-color -- --color green
```
## Configure Environment Variables in Applications
![image](https://github.com/rlarudgkswkd/CKA_study/assets/48428850/d12abf5b-1cff-44b2-958b-0e42944e2cad)
![image](https://github.com/rlarudgkswkd/CKA_study/assets/48428850/d0074cb5-b6f1-4403-9b00-a2ac00476834)

## Configuring ConfigMaps in Applications
### create
  - imperative
  ```
  kubectl create configmap
  kubectl get configmap
  kubectl describe configmaps
  ```
  ![image](https://github.com/rlarudgkswkd/CKA_study/assets/48428850/b86b498f-93d0-4997-8f70-44a80a969706)

  - Declarative
  ```
    
  ```
  ![image](https://github.com/rlarudgkswkd/CKA_study/assets/48428850/8d4aa259-1f0d-45dd-836e-5184eb2749c5)

### ConfigMap in Pods(inject the configMap)
![image](https://github.com/rlarudgkswkd/CKA_study/assets/48428850/d7755076-51ee-4f15-b6fa-90143f717794)


