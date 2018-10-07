---
layout: post
title: "Install Minikube and Kubernetes on ubuntu 18"
date: 2018-10-05 11:58:38 +0200
categories: kubernetes
---
Minikube is a tool that allow you to run Kubernetes locally, running a single-node Kubernetes cluster inside a VM on your local machine.It's the ideal tool for move the firsts step into the wide world of Kubernetes.

### Install Minikube:
Before to install minikube you need to install an Hypervisor, in my case i will install [Virtualbox](https://www.virtualbox.org/wiki/Linux_Downloads).

download and install with curl:
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.30.0/minikube-linux-amd64 && chmod +x minikube && sudo cp minikube /usr/local/bin/ && rm minikube
```

### Install kubectl
kubectl is the Kubernetes command-line tool to deploy and manage applications on Kubernetes.
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
```
Make the kubectl binary executable.
```
chmod +x ./kubectl
```
Move into the path:
```
sudo mv ./kubectl /usr/local/bin/kubectl
```

### Test minikube and Kubectl:
start minikube:
```
minikube start
```
check if minikube running:
```
minikube status
```
create a pods on minikube with kubectl:
```
kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.4 --port=8080
```

expose the pods:
```
kubectl expose deployment hello-minikube --type=NodePort
```

get the url:
```
minikube service hello-minikube --url
```

stop minikube:
```
minikube stop
```
