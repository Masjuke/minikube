
Steps on this page require that the Heapster service is running in your cluster. But if you don’t have Heapster running, you can do most of the steps, and it won’t be a problem if you skip the Heapster steps.

If you are running minikube, run the following command to enable heapster:

minikube addons enable heapster

To see whether the Heapster service is running, enter this command:
kubectl get services --namespace=kube-system

If the Heapster service is running, it shows in the output:

```shell
kubectl get services --namespace=kube-system
NAME                   TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)             AGE
default-http-backend   NodePort    10.0.0.53    <none>        80:30001/TCP        2d
heapster               NodePort    10.0.0.127   <none>        80:30599/TCP        23s
kubernetes-dashboard   NodePort    10.0.0.117   <none>        80:30000/TCP        2d
monitoring-grafana     NodePort    10.0.0.119   <none>        80:30002/TCP        23s
monitoring-influxdb    ClusterIP   10.0.0.58    <none>        8083/TCP,8086/TCP   22s
registry               ClusterIP   10.0.0.231   <none>        80/TCP              2d
```
  
Create a namespace

Create a namespace so that the resources you create in this exercise are isolated from the rest of your cluster.
kubectl create namespace mem-example
Specify a memory request and a memory limit

To specify a memory request for a Container, include the resources:requests field in the Container’s resource manifest. To specify a memory limit, include resources:limits.

In this exercise, you create a Pod that has one Container. The Container has a memory request of 100 MiB and a memory limit of 200 MiB. Here’s the configuration file for the Pod:

      memory-request-limit.yaml  Copy memory-request-limit.yaml to clipboard
      apiVersion: v1
      kind: Pod
      metadata:
        name: memory-demo
      spec:
        containers:
        - name: memory-demo-ctr
          image: vish/stress
          resources:
            limits:
              memory: "200Mi"
            requests:
              memory: "100Mi"
          args:
          - -mem-total
          - 150Mi
          - -mem-alloc-size
          - 10Mi
          - -mem-alloc-sleep
          - 1s
          
In the configuration file, the args section provides arguments for the Container when it starts. The -mem-total 150Mi argument tells the Container to attempt to allocate 150 MiB of memory.


Create the Pod:
kubectl create -f https://k8s.io/docs/tasks/configure-pod-container/memory-request-limit.yaml --namespace=mem-example
Verify that the Pod’s Container is running:

kubectl get pod memory-demo --namespace=mem-example
View detailed information about the Pod:

kubectl get pod memory-demo --output=yaml --namespace=mem-example
The output shows that the one Container in the Pod has a memory request of 100 MiB and a memory limit of 200 MiB.

      ...
      resources:
        limits:
          memory: 200Mi
        requests:
          memory: 100Mi
      ...

Start a proxy so that you can call the Heapster service:

kubectl proxy
In another command window, get the memory usage from the Heapster service:

`curl http://localhost:8001/api/v1/proxy/namespaces/kube-system/services/heapster/api/v1/model/namespaces/mem-example/pods/memory-demo/metrics/memory/usage`

The output shows that the Pod is using about 162,900,000 bytes of memory, which is about 150 MiB. This is greater than the Pod’s 100 MiB request, but within the Pod’s 200 MiB limit.

      {
       "timestamp": "2017-06-20T18:54:00Z",
       "value": 162856960
      }

Delete your Pod:

`kubectl delete pod memory-demo --namespace=mem-example`

Exceed a Container’s memory limit

A Container can exceed its memory request if the Node has memory available. But a Container is not allowed to use more than its memory limit. If a Container allocates more memory than its limit, the Container becomes a candidate for termination. If the Container continues to consume memory beyond its limit, the Container is terminated. If a terminated Container is restartable, the kubelet will restart it, as with any other type of runtime failure.

In this exercise, you create a Pod that attempts to allocate more memory than its limit. Here is the configuration file for a Pod that has one Container. The Container has a memory request of 50 MiB and a memory limit of 100 MiB.

      memory-request-limit-2.yaml  Copy memory-request-limit-2.yaml to clipboard
      apiVersion: v1
      kind: Pod
      metadata:
        name: memory-demo-2
      spec:
        containers:
        - name: memory-demo-2-ctr
          image: vish/stress
          resources:
            requests:
              memory: "50Mi"
            limits:
              memory: "100Mi"
          args:
          - -mem-total
          - 250Mi
          - -mem-alloc-size
          - 10Mi
          - -mem-alloc-sleep
          - 1s
          
          
In the configuration file,in the args section, you can see that the Container will attempt to allocate 250 MiB of memory, which is well above the 100 MiB limit.

Create the Pod:

`kubectl create -f https://k8s.io/docs/tasks/configure-pod-container/memory-request-limit-2.yaml --namespace=mem-example`


View detailed information about the Pod:

`kubectl get pod memory-demo-2 --namespace=mem-example`

At this point, the Container might be running, or it might have been killed. If the Container has not yet been killed,
repeat the preceding command until you see that the Container has been killed:

    NAME            READY     STATUS      RESTARTS   AGE
    memory-demo-2   0/1       OOMKilled   1          24s

Get a more detailed view of the Container’s status:

`kubectl get pod memory-demo-2 --output=yaml --namespace=mem-example`

The output shows that the Container has been killed because it is out of memory (OOM).
lastState:

         terminated:
           containerID: docker://65183c1877aaec2e8427bc95609cc52677a454b56fcb24340dbd22917c23b10f
           exitCode: 137
           finishedAt: 2017-06-20T20:52:19Z
           reason: OOMKilled
           startedAt: null
           
The Container in this exercise is restartable, so the kubelet will restart it. Enter this command several times to see that the Container gets repeatedly killed and restarted:

`kubectl get pod memory-demo-2 --namespace=mem-example`

The output shows that the Container gets killed, restarted, killed again, restarted again, and so on:

NAME            READY     STATUS      RESTARTS   AGE
memory-demo-2   0/1       OOMKilled   1          37s

`$ kubectl get pod memory-demo-2 --namespace=mem-example`

      NAME            READY     STATUS    RESTARTS   AGE
      memory-demo-2   1/1       Running   2          40s

View detailed information about the Pod’s history:

`kubectl describe pod memory-demo-2 --namespace=mem-example`

The output shows that the Container starts and fails repeatedly:

      ... Normal  Created   Created container with id 66a3a20aa7980e61be4922780bf9d24d1a1d8b7395c09861225b0eba1b1f8511
      ... Warning BackOff   Back-off restarting failed container

View detailed information about your cluster’s Nodes:

kubectl describe nodes
The output includes a record of the Container being killed because of an out-of-memory condition:

`Warning OOMKilling  Memory cgroup out of memory: Kill process 4481 (stress) score 1994 or sacrifice child`

Delete your Pod:

`kubectl delete pod memory-demo-2 --namespace=mem-example`

Specify a memory request that is too big for your Nodes

Memory requests and limits are associated with Containers, but it is useful to think of a Pod as having a memory request and limit. The memory request for the Pod is the sum of the memory requests for all the Containers in the Pod. Likewise, the memory limit for the Pod is the sum of the limits of all the Containers in the Pod.

Pod scheduling is based on requests. A Pod is scheduled to run on a Node only if the Node has enough available memory to satisfy the Pod’s memory request.

In this exercise, you create a Pod that has a memory request so big that it exceeds the capacity of any Node in your cluster. Here is the configuration file for a Pod that has one Container. The Container requests 1000 GiB of memory, which is likely to exceed the capacity of any Node in your cluster.

      memory-request-limit-3.yaml  Copy memory-request-limit-3.yaml to clipboard
      apiVersion: v1
      kind: Pod
      metadata:
        name: memory-demo-3
      spec:
        containers:
        - name: memory-demo-3-ctr
          image: vish/stress
          resources:
            limits:
              memory: "1000Gi"
            requests:
              memory: "1000Gi"
          args:
          - -mem-total
          - 150Mi
          - -mem-alloc-size
          - 10Mi
          - -mem-alloc-sleep
          - 1s
          
Create the Pod:

`kubectl create -f https://k8s.io/docs/tasks/configure-pod-container/memory-request-limit-3.yaml --namespace=mem-example`

View the Pod’s status:

`kubectl get pod memory-demo-3 --namespace=mem-example`

The output shows that the Pod’s status is PENDING. That is, the Pod has not been scheduled to run on any Node, and it will remain in the PENDING state indefinitely:

      kubectl get pod memory-demo-3 --namespace=mem-example
      NAME            READY     STATUS    RESTARTS   AGE
      memory-demo-3   0/1       Pending   0          25s
      
View detailed information about the Pod, including events:

`kubectl describe pod memory-demo-3 --namespace=mem-example`

The output shows that the Container cannot be scheduled because of insufficient memory on the Nodes:
Events:

 ` ...  Reason            Message
       ------            -------
  ...  FailedScheduling  No nodes are available that match all of the following predicates:: Insufficient memory (3).`
  
Memory units

The memory resource is measured in bytes. You can express memory as a plain integer or a fixed-point integer with one of these suffixes: `E, P, T, G, M, K, Ei, Pi, Ti, Gi, Mi, Ki.` For example, the following represent approximately the same 

value:
`128974848, 129e6, 129M , 123Mi`

Delete your Pod:

`kubectl delete pod memory-demo-3 --namespace=mem-example`

If you don’t specify a memory limit

If you don’t specify a memory limit for a Container, then one of these situations applies:

The Container has no upper bound on the amount of memory it uses. The Container could use all of the memory available on the Node where it is running.

The Container is running in a namespace that has a default memory limit, and the Container is automatically assigned the default limit. Cluster administrators can use a LimitRange to specify a default value for the memory limit.
Motivation for memory requests and limits

By configuring memory requests and limits for the Containers that run in your cluster, you can make efficient use of the memory resources available on your cluster’s Nodes. By keeping a Pod’s memory request low, you give the Pod a good chance of being scheduled. By having a memory limit that is greater than the memory request, you accomplish two things:

* The Pod can have bursts of activity where it makes use of memory that happens to be available.
* The amount of memory a Pod can use during a burst is limited to some reasonable amount.
Clean up

Delete your namespace. This deletes all the Pods that you created for this task:

`kubectl delete namespace mem-example`
