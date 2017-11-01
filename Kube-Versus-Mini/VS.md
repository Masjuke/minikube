# Kubernetes & Minikube


Currently the most popular platform orchestration for managing containerized application is kubernetes among other platform for several reasons of course, but also cannot be denied that kubernetes are more complex to setup. below are some features of kubernetes.

### Why need Kubernetes

* Real production apps span multiple containers. Those containers must be deployed across multiple server hosts. Kubernetes gives you the orchestration and management capabilities required to deploy containers, at scale, for these workloads. Kubernetes orchestration allows you to build application services that span multiple containers, schedule those containers across a cluster, scale those containers, and manage the health of those containers over time.

* Kubernetes also needs to integrate with networking, storage, security, telemetry and other services to provide a comprehensive container infrastructure.


![kubernetes-diagram-902x416](https://user-images.githubusercontent.com/32785359/32269852-3e06b6b8-bf26-11e7-9f5a-ca2209e1e455.png)


* Kubernetes fixes a lot of common problems with container proliferation—sorting containers together into a ”pod.” Pods add a layer of abstraction to grouped containers, which helps you schedule workloads and provide necessary services—like networking and storage—to those containers. Other parts of Kubernetes help you load balance across these pods and ensure you have the right number of containers running to support your workloads.


While kubernetes purpose to runs on production what if we need to run in local machine to test kubernetes ?? don't worries you can use kubernetes as an alternatives


Minikube is a tool that makes it easy to run Kubernetes locally. Minikube runs a SINGLE-NODES Kubernetes cluster inside a VM (on Virtualbox for example) on your laptop for users looking to try out Kubernetes or develop with it day-to-day. Internally, minikube runs a single Go binary (named localkube), which runs all the kubernetes’ components. The result is a local kubernetes endpoint that you can use with the kubectl client.

Minikube supports kubernetes features such as 

```shell
DNS, Dashboards, CNI, NodePorts, ConfigMaps and Secrets and so on.
```

The minikube VM runs Docker, but supports also rkt container engine.
Possible to reuse the minikube’s built-in Docker daemon; as this means you don’t have to build a docker registry on your host machine and push the image into it – you can just build inside the same docker daemon as minikube which speeds up local experiments.





