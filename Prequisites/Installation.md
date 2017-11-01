
## Installing Minikube on local machine 

* Minikube is a tool as a replacement for kubernetes if we want to use in local machine, some features might be not provided compare to real kubernetes.

* It is also the recommended method for creating a local, single-node Kubernetes cluster for development and testing. Setup is completely automated and doesn’t require a cloud provider account.

* This installation is intended to Mac laptop since this is the most apropriate environment instead using remote linux server or vagrant at my place. but don't worries we'll not be losing the main purpose of why we do this test :

```shell
    Deploy container 
    Volume
    Scheduler
    Replicas
    CPUs
    Memory and so on
```

* In order to work with Minikube, we should have Kubectl and Minikube installed + some virtualization drivers.
For OS X, install xhyve driver, VirtualBox, or VMware Fusion, then Kubectl and Minkube


```shell

* Kubectl
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectlwebserver-2022867364-r5zzl

chmod +x ./kubectl

sudo mv ./kubectl /usr/local/bin/kubectl

* Minikube
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.21.0/minikube-darwin-amd64

chmod +x minikube && sudo mv minikube /usr/local/bin/
```


