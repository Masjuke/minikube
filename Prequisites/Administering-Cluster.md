
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


 By default minikube has three namespace which is default, system and public
 
       $ kubectl get namespace
      NAME          STATUS    AGE
      default       Active    1d
      kube-public   Active    1d
      kube-system   Active    1d
      testing       Active    23h
      
 ```shell 
 $ kubectl create namespace testing
 ```
 

I added namespace testing at the firstime launched. by the way namespace are multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces.

 * we are going to use new namespace instead existing cluster namespace for now
 * create Json file to create new namespace
 * Pull images using kubectl and deploy to namespace we just created
 
There's not much we can do to manage cluster in minikube cause limited by feature but it will be enough to jump in to kubernetes atmosphere

### Create namespace using YAML file

    
    # Create folder minikube that contains YAML file
    $ mkdir minikube


    # Get inside to minikunbe folder
    $ cd minikube

    # Create YAML file
    $ touch namespace-dev.yaml




```shell
# YAML file to be executed to minikube dashboard to create new namespace development for our testing
# All running image or container will be done in this namespace
{
  "kind": "Namespace",
  "apiVersion": "v1",
  "metadata": {
    "name": "development",
    "labels": {
      "name": "development"
    }
  }
}
```





