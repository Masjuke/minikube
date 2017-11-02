
## Administering cluster

On first chapter we allready installed minikube, get known with nodes info and cluster also run minikube dashboard for to makesure minikube is run as expected

Kubernetes is a complex sets of configuration so we need very clearly i the beginning to manage orchestration on it, there are several things need to consider before proceed 

```shell
Planning a cluster
Managing a cluster
Securing a cluster
Securing the kubelet
Optional Cluster Services
```

### Planning

* Do you just want to try out Kubernetes on your computer, or do you want to build a high-availability, multi-node cluster? Choose distros best suited for your needs.
* If you are designing for high-availability, learn about configuring clusters in multiple zones.
* Will you be using a hosted Kubernetes cluster, such as Google Container Engine (GKE), or hosting your own cluster?
* Will your cluster be on-premises, or in the cloud (IaaS)? Kubernetes does not directly support hybrid clusters. Instead, you can set up multiple clusters.
* If you are configuring Kubernetes on-premises, consider which networking model fits best. One option for custom networking is OpenVSwitch GRE/VxLAN networking, which uses OpenVSwitch to set up networking between pods across Kubernetes nodes.
* Will you be running Kubernetes on “bare metal” hardware or on virtual machines (VMs)?
* Do you just want to run a cluster, or do you expect to do active development of Kubernetes project code? If the latter, choose a actively-developed distro. Some distros only use binary releases, but offer a greater variety of choices.

### Manage Cluster

* Managing a cluster describes several topics related to the lifecycle of a cluster: creating a new cluster, upgrading your cluster’s master and worker nodes, performing node maintenance (e.g. kernel upgrades), and upgrading the Kubernetes API version of a running cluster.

### Securing a cluster

```shell
Certificates describes the steps to generate certificates using different tool chains.

Kubernetes Container Environment describes the environment for Kubelet managed containers on a Kubernetes node.

Controlling Access to the Kubernetes API describes how to set up permissions for users and service accounts.

Authenticating explains authentication in Kubernetes, including the various authentication options.

Authorization is separate from authentication, and controls how HTTP calls are handled.

Using Admission Controllers explains plug-ins which intercepts requests to the Kubernetes API server after authentication and authorization.

Using Sysctls in a Kubernetes Cluster describes to an administrator how to use the sysctl command-line tool to set kernel parameters .

Auditing describes how to interact with Kubernetes’ audit logs.
```


Above definition to consider is for kubernetes, since we are going to test in local machine environment don't have to worries about it.


* Start Minikube on your local machine

        $ minikube start
      Starting local Kubernetes v1.8.0 cluster...
      Starting VM...
      Getting VM IP address...
      Moving files into cluster...
      Setting up certs...
      Connecting to cluster...
      Setting up kubeconfig...
      Starting cluster components...
      Kubectl is now configured to use the cluster.
      
      

```shell
# Run dashboard to have general overview
$ Minikube dashboard
```


<img width="1432" alt="screen shot 2017-11-02 at 1 48 15 pm" src="https://user-images.githubusercontent.com/32785359/32313259-8d711a3c-bfd4-11e7-80f8-39b3fb7291f4.png">



 
 ```shell
 #Limiting Minikube memory
 
 MJuke-MacBook-Pro% minikube config set  memory 3000
These changes will take effect upon a minikube delete and then a minikube start
```
