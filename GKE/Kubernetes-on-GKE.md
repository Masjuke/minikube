
## Kubernetes on GKE

Why do we need to run kubernetes on google container engine instead of other platform, well I am not an kubernetes expert but since kubernetes is came from google product there's a plus point I believed.



 * kubernetes master — Don’t have to worry about Kubernetes master going down. GKE guarantees master node’s uptime.

 * etcd — Don’t have to learn etcd. Don’t have to know how many etcd’s should we have to spin 3/5/7 for my cluster? Should my master and etcd be on the same node? we don’t have to answer these questions.GKE guarantees etcd doesn’t go down and it manages etcd.

 * Container networking — Should I have to use weave,flannel ? How does it scale and work ? Need not worry about container networking on GKE. It is managed. https://cloud.google.com/compute/docs/networking#routing

 * IAM — kubernetes 1.3 has access to RBAC in Alpha. But how do I give access to the other members in my team? Should I share certificate to each Dev? How do I manage authorization/authentication? GKE has builtin authentication and authorization. https://cloud.google.com/container-engine/docs/iam-integration

 * Autoscaling — In GKE autoscaling cluster is as simple as gcloud container clusters create NAME — zone ZONE — num-nodes=30 \
 — enable-autoscaling — min-nodes=15 — max-nodes=50 https://cloud.google.com/containerengine/docs/clusters/operations#create_a_cluster_with_autoscaling . This will spin up to 50 Google Compute Engine (GCE) instances,if your cluster has to grow based on load.
 
* Upgrading — Kubernetes has a release almost every 3–4 months and you can’t upgrade the master inplace. The only way to upgrade is to spin up a new cluster with the latest release and then move your pods to this new cluster. In GKE the master is upgraded automatically for you and your responsibility is to upgrade the nodes which is again just one command gcloud container clusters upgrade CLUSTER_NAME \
 [ — cluster-version=X.Y.Z]
 
* Heapster — Kubernetes heapster has built in support for Google Cloud Monitoring

* Logging — GKE has built in support for cluster level logging to Google Cloud Logging which can be exported to BigQuery.
And here is the twitter conversation on this topic with the others https://twitter.com/milesward/status/771425568948531200

* AWS doesn’t have a managed Kubernetes installation like GKE does. GKE, on the other hand, has Google backing and integrates with all of Google’s other tooling. It comes with built-in logging, log management and monitoring at both the host and container levels. Unlike AWS, it can give you automatic autoscaling, automatic hardware management and automatic version updates. It generally gives you a production-ready cluster with a more batteries-included approach than if you were building everything by hand on AWS.

* With GKE, you get a production-ready Kubernetes cluster with all the necessary tooling, along with ongoing support needed to make sure your packages and versions stay current. GKE takes care of security defaults for you and integrates with other Google services. This combination requires less overhead in managing the cluster and makes it more seamless to use in the long run.
