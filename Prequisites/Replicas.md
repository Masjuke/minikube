### Scale replicas of a container

* ReplicaSet ensures that a specified number of pod replicas are running at any given time. However, a Deployment is a higher-level concept that manages ReplicaSets and provides declarative updates to pods along with a lot of other useful features. Therefore, we recommend using Deployments instead of directly using ReplicaSets, unless you require custom update orchestration or don’t require updates at all.
This actually means that you may never need to manipulate ReplicaSet objects: use a Deployment instead, and define your application in the spec section.



```shell
# Scale replica for container with specific namespace
kubectl scale --replicas=4 --namespace=development  deployment/mynginx
deployment "mynginx" scaled
```


```shell
# Scale replica for container with default namespace
kubectl scale --replicas=4 deployment/mynginx
deployment "mynginx" scaled
```


                % kubectl get replicaset
                NAME                  DESIRED   CURRENT   READY     AGE
                my-nginx-69d54fff74   4         4         4         1h
                mynginx-59f44f8b8b    1         1         1         1h
                
                # replicaset based on namespace
                MJuke-MacBook-Pro% kubectl get replicaset --namespace=development
                NAME                 DESIRED   CURRENT   READY     AGE
                mynginx-59f44f8b8b   4         4         4         1h

                MJuke-MacBook-Pro% kubectl get replicaset --namespace=default
                NAME                  DESIRED   CURRENT   READY     AGE
                my-nginx-69d54fff74   4         4         4         1h
                mynginx-59f44f8b8b    1         1         1         1h
