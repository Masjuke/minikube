### Deploy app on minikube 

* We can deploy app using kubectl command line 
* Or using docker through docker daemon env inside minikube
* pull image from registry gcr.io or from docker hub
* Set container to run on new namespace we just created
* Deploy apps service
* set replicas

```shell
MJuke-MacBook-Pro% kubectl run mynginx --image=nginx
deployment "mynginx" created
```

let's have take a look 


    kubectl get pods
    NAME                       READY     STATUS    RESTARTS   AGE
    mynginx-59f44f8b8b-mwrbp   1/1       Running   0          6m
    MJuke-MacBook-Pro% kubectl describe pods
    Name:           mynginx-59f44f8b8b-mwrbp
    Namespace:      default
    Node:           minikube/192.168.99.100
    Start Time:     Thu, 02 Nov 2017 14:49:17 +0700
    Labels:         pod-template-hash=1590094646
                    run=mynginx

mynginx is created as describe above and using default namespace because we didn't set specifically

```shell
MJuke-MacBook-Pro% kubectl run my-nginx --image=nginx --namespace=development
deployment "my-nginx" created
```

* kubectl get pods 

<img width="950" alt="screen shot 2017-11-02 at 3 09 25 pm" src="https://user-images.githubusercontent.com/32785359/32315724-e004fd94-bfdf-11e7-8a59-04d429890936.png">


* New app deployed to cluster namespace development based on image from minikube dashboard

PODs is part of namespace development and we just added 1 container which is "mynginx" inside the pods under the development namespace.

Minikube onlh have one nodes but can arrange virtual cluster using namespaces, but we can put container as much as we can to the pods.


