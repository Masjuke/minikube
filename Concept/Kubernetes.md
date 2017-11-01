## Kubernetes



    Kubernetes is an opensource container orchestration for docker container application, it can manage hundreds to   thousands

    container on multiple host and provide mechanisme, orchestration, maintenance, autoscaling of container applications.

    On a high level concept of kubernetes there's a tree foundation which stands as a concept of running kubernetes environmen

    you can see on kubernetes official website for their concept https://kubernetes.io

    A typical application would have a cluster of containers across multiple hosts. For example, your web tier (Apache or 

    Undertow) might run on a set of containers. Similarly, your application tier (WildFly) would run on a different set of
    containers. 

    The web tier would need to delegate the request to application tier. In some cases, or at least to begin with,

    you may have your web and application server packaged together in the same set of containers. The database tier would

    generally run on a separate tier anyway. These containers would need to talk to each other. Using any of the solutions 

    mentioned above would require scripting to start the containers, and monitoring/bouncing if something goes down. Kubernetes

    does all of that for the user after the application state has been defined.

    Kubernetes is cloud-agnostic. This allows it run on public, private or hybrid clouds. Any cloud provider such as Google

    Cloud Engine. OpenShift v3 is going to be based upon Docker and Kubernetes. It can even run on a variety of hypervisors,

    such as VirtualBox.



### Key concepts of Kubernetes

At a very high level, there are three key concepts:

* Pods are the smallest deployable units that can be created, scheduled, and managed. Its a logical collection of containers that belong to an application.

* Master is the central control point that provides a unified view of the cluster. There is a single master node that control multiple minions.

* Minion is a worker node that run tasks as delegated by the master. Minions can run one or more pods. It provides an application-specific “virtual host” in a containerized environment.




![kubernetes-architecture-1024x793](https://user-images.githubusercontent.com/32785359/32266809-6a87419a-bf1b-11e7-9bba-44fa33281f42.png)


