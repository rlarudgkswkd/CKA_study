# Install k8s the kubeadm way

## Introduction
### Step
1. provision node and set the master , worker nodes
2. install container runtime on the host ex. containerd
3. initialize the master server
4. set the pod network
5. join the worker node to the master node

## Setting up
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
- ```ps -p 1```
- kubeadm init
- ```
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config      
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
  ```
- installing addons
  ```
  kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
  ```
- Reference links:
  - https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#-installation
  - https://github.com/weaveworks/weave/releases
