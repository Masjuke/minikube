## Create namespace


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

    # we can see new namespace added with labels development
    kubectl get namespace --show-labels
    NAME          STATUS    AGE       LABELS
    default       Active    1d        <none>
    development   Active    25m       name=development
    kube-public   Active    1d        <none>
    kube-system   Active    1d        <none>
    testing       Active    1d        <none>


