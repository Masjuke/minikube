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

Let's create another one

We can manage the k8s cluster with kubectl command. Let's see nodes of our cluster.

```bash
➜  kubectl get nodes
NAME       STATUS    AGE
minikube   Ready     05d
```

A very detailed information for a given node can be achived via `kubectl describe node minikube`. You can also start a fancy Web dashboard ! `kubectl dashboard` will automatically open in your default Web browser, to get only the URL, add `--url`.

Let's run a deployment using Docker image `darek/goweb:1.0` and exposes port 8080. 

```bash
➜ kubectl run webserver --image=juke/webserver:1.0 --port=8080
```
You can add labels to pods, for a faster search and discoverability. Let's add labels to all pods we have (for now we have only one). Labels are great to select pods you want if you have many of them.

```bash
➜ kubectl label pods --all app=webserver
➜ kubectl label pods --all environement=production
➜ kubectl label pods --all release=stable
```

The same effect can be achieved by using a YAML file: `kubectl create -f webserver_deployment.yml`. 

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: 
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: webserver
        release: stable
        environment: production
    spec:
      containers:
      - name: goweb
        image: juke/webserver:1.0
        ports:
        - containerPort: 8080
```

Remember that kuberneter needs to pull the image, so the container may not beed provisioned right away.
Let's see pods:

```bash
➜ kubectl get pods
NAME                    READY     STATUS              RESTARTS   AGE
webserver-907329357-dnycx   0/1       ContainerCreating   0          2m
```
And now by labels:

```
➜ kubectl get pods -l app=goweb --show-labels
NAME                     READY     STATUS              RESTARTS   AGE       LABELS
webserver-907329357-dnycx    0/1       ContainerCreating   0          13m       app=webserver,pod-template-hash=2461254821,stage=testing,what=test
```

`ContainerCreating` says that the container is in progress. Use a `describe` command to see detailed information. 

```bash
➜ kubectl describe pod webserver-907329357-dnycx 
Name:		webserver-907329357-dnycx 
Namespace:	default
Node:		minikube/192.168.99.100
Start Time:	Sun, 05 Nov 2017 07:26:08 +0100
Labels:		pod-template-hash=907329357
		    run=webserver
Status:		Pending
IP:
Controllers:	ReplicaSet/webserver-907329357
Containers:
  webserver:
    Container ID:
    Image:			juke/webserver:1.0
    Image ID:
    Port:			8080/TCP
    State:			Waiting
      Reason:			ContainerCreating
    Ready:			False
    Restart Count:		0
    Environment Variables:	<none>
Conditions:
  Type		Status
  Initialized 	True
  Ready 	False
  PodScheduled 	True
Volumes:
  default-token-f3ech:
    Type:	Secret (a volume populated by a Secret)
    SecretName:	default-token-f3ech
QoS Tier:	BestEffort
Events:
  FirstSeen	LastSeen	Count	From			SubobjectPath		Type		Reason		Message
  ---------	--------	-----	----			-------------		--------	------		-------
  2m		2m		1	{default-scheduler }				Normal		Scheduled	Successfully assigned webserver-907329357-jiau5 to minikube
  2m		2m		1	{kubelet minikube}	spec.containers{webserver}	Normal		Pulling		pulling image "juke/webserver:1.0"
```
You can that the pod is pulling the image, Events section is very helpful. After some time the pod, and the deployment should be ready. 

```bash 
➜ kubectl get deployments
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
webserver     1         1         1            1           3m
```

Remember that in order to get a detailed insight into pods and deployments with a `describe command`. You can also see logs from a pod with a `kubectl logs PODNAME` command.

```bash
➜ kubectl logs webserver-907329357-dnycx 
2016/12/12 06:33:26 Starting goweb 1.0
```


Let's define a service, which will group our pods, expose a port and provide discoverability. Type `NodePort` says that the port will be exposed.

```bash
➜ kubectl expose deployment webserver --type=NodePort
➜ kubectl get services
NAME         CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
webserver        10.0.0.191   <nodes>       8080/TCP   9s
kubernetes   10.0.0.1     <none>        443/TCP    76d
```

In order to access the service to its exposed port, you can use minikube's helped command: ` minikube service webserver`. It will open your browser right to the address:port of the service. If you have the httpie tool installed (`pip install httpie`), you use this oneliner:

```bash
http $(minikube service  webserver --url)
HTTP/1.1 200 OK
Content-Length: 130
Content-Type: text/plain; charset=utf-8
Date: Sun, 05 Nov 2017 17:35:09 

hi from d75e0a562f0f ! version 1.0
Accept-Encoding:[gzip, deflate]
Accept:[*/*]
User-Agent:[HTTPie/0.9.2]
Connection:[keep-alive]
```

Let's now scale the service to 4 pods:

```bash
➜ kubectl scale --replicas=4 deployment/webserver
```
Now, when you check the deployment, you should see 4 pods running:

```
➜ kubectl get deployment
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
webserver     4         4         4            4           33m
```

Let's check if service is keeping our pods safe, let's kill one:

```
➜ kubectl get pods
NAME                    READY     STATUS    RESTARTS   AGE
webserver-988856142-0dc3a   1/1       Running   0          2m
webserver-988856142-dxqvb   1/1       Running   0          1m
webserver-988856142-jbdyl   1/1       Running   0          1m
webserver-988856142-xhm5y   1/1       Running   0          6m
➜ kubectl delete pod webserver-988856142-xhm5y
pod "webserver-988856142-xhm5y" deleted
➜ kubectl get pods
NAME                    READY     STATUS              RESTARTS   AGE
webserver-988856142-0dc3a   1/1       Running             0          3m
webserver-988856142-dxqvb   1/1       Running             0          2m
webserver-988856142-xhm5y   1/1       Running             0          7m
webserver-988856142-z1p7b   0/1       ContainerCreating   0          1s
``` 

The above listing shows that immediately k8s healed our service, taking it back to intended 4 pods running. Let's now do an update:

```
➜ kubectl set image deployment/webserver webserver=juke/webserver:2.0
```

Go to the Web browser and check if a container is anouncing a 2.0 version:

```bash
http $(minikube service  webserver --url)
HTTP/1.1 200 OK
Content-Length: 130
Content-Type: text/plain; charset=utf-8
Date: Sun, 05 Nov 2017 17:35:09 

hi from e95e0d562e0a ! version 2.0
Accept-Encoding:[gzip, deflate]
Accept:[*/*]
User-Agent:[HTTPie/0.9.2]
Connection:[keep-alive]
```
You can also verify if a pod is indeed updated to 2.0 by executing `kubectl describe pod PODID` or accessing the logs, where you should see version 2.0: `2017/11/05 17:51:38 Starting webserver 2.0`. 


We can now delete the cluster and stop the minikube.

```bash
➜ kubectl delete service,deployment webserver
service "webserver" deleted
deployment "webserver" deleted
➜ minikube stop
```

