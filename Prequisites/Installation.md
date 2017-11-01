
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

* macOS
    * [xhyve driver](https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#xhyve-driver), [VirtualBox](https://www.virtualbox.org/wiki/Downloads), or [VMware Fusion](https://www.vmware.com/products/fusion)


```shell

# Downloading Kubectl for minikube command line
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectlwebserver-2022867364-r5zzl

# Grant access for kubectl
chmod +x ./kubectl

# move kubectl to /usr/ PATH
sudo mv ./kubectl /usr/local/bin/kubectl

# Downloading Minikube
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.21.0/minikube-darwin-amd64

# Gives access and move to usr PATH
chmod +x minikube && sudo mv minikube /usr/local/bin/
```

```shell
# After all dependencies is done then we can start minikube to use virtualbox driver

MJuke-MacBook-Pro% minikube start --vm-driver=virtualbox
Starting local Kubernetes v1.8.0 cluster...
Starting VM...
Getting VM IP address...
Moving files into cluster...
Setting up certs...
Connecting to cluster...
Setting up kubeconfig...
Starting cluster components...
Kubectl is now configured to use the cluster.
```

```shell
# Minikube is starting and now let see which version do we used
MJuke-MacBook-Pro% minikube version
minikube version: v0.23.0

# Check minikube status correctly 
MJuke-MacBook-Pro% minikube status
minikube: Running
cluster: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.100

# Check node version
MJuke-MacBook-Pro% kubectl get nodes
NAME       STATUS    ROLES     AGE       VERSION
minikube   Ready     <none>    14h       v1.8.0

# Run kubectl cluster-info to finds out running IP
MJuke-MacBook-Pro% kubectl cluster-info
Kubernetes master is running at https://192.168.99.100:8443
```

### Dashboard

To access the [Kubernetes Dashboard](http://kubernetes.io/docs/user-guide/ui/), run this command in a shell after starting Minikube to get the address:

```shell
MJuke-MacBook-Pro% minikube dashboard
Opening kubernetes dashboard in default browser...
```

<img width="1432" alt="screen shot 2017-11-02 at 1 27 05 am" src="https://user-images.githubusercontent.com/32785359/32290571-0f07e070-bf6d-11e7-9c63-479268dbeaeb.png">




