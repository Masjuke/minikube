
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



### Docker daemon

To be able to work with the docker daemon on your mac/linux host use the docker-env command in your shell:

```shell
eval $(minikube docker-env)
```

You should now be able to use docker on the command line on your host mac/linux machine talking to the docker daemon inside the minikube VM:

```shell
docker ps
```

```shell
MJuke-MacBook-Pro% eval $(minikube docker-env)
```

```shell
# Run docker ps inside docker daemon on minikube
MJuke-MacBook-Pro% docker ps
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS              PORTS                                                                NAMES
a0fc842d3c41        gcr.io/google_containers/heapster-influxdb-amd64      "influxd --config ..."   42 minutes ago      Up 42 minutes                                                                            k8s_influxdb_influxdb-grafana-qz4b4_kube-system_3f53d80a-beb5-11e7-aa72-080027b8167f_3
5087a5a884a5        coredns/coredns                                       "/coredns -conf /e..."   43 minutes ago      Up 43 minutes                                                                            k8s_coredns_coredns-6b4fd7784-5w7s7_kube-system_3fb4ac44-beb5-11e7-aa72-080027b8167f_1
5d71ba8d141d        registry                                              "/entrypoint.sh /e..."   43 minutes ago      Up 43 minutes                                                                            k8s_registry_registry-xdgmt_kube-system_3fc74cd7-beb5-11e7-aa72-080027b8167f_1
cdb47308e11a        gcr.io/google_containers/pause-amd64:3.0              "/pause"                 43 minutes ago      Up 43 minutes                                                                            k8s_POD_registry-xdgmt_kube-system_3fc74cd7-beb5-11e7-aa72-080027b8167f_1
6ea23c14dd34        gcr.io/google_containers/heapster                     "/heapster --sourc..."   43 minutes ago      Up 43 minutes                                                                            k8s_heapster_heapster-shnpp_kube-system_3f4431b5-beb5-11e7-aa72-080027b8167f_1
b21f959bbd81        gcr.io/google_containers/pause-amd64:3.0              "/pause"                 43 minutes ago      Up 43 minutes                                                                            k8s_POD_heapster-shnpp_kube-system_3f4431b5-beb5-11e7-aa72-080027b8167f_1
94a3399c8fe9        gcr.io/google_containers/kubernetes-dashboard-amd64   "/dashboard --inse..."   43 minutes ago      Up 43 minutes                                                                            k8s_kubernetes-dashboard_kubernetes-dashboard-8jpgr_kube-system_3f210025-beb5-11e7-aa72-080027b8167f_1
```



