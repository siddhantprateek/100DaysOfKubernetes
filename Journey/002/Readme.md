# Day 2:

Inside Kubernetes cluster:

`Main Node`
![Group 1](https://user-images.githubusercontent.com/43869046/119663858-65d43800-be50-11eb-8c10-24b410bb113b.png)


#### [API Server](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)
API server is responsible for interaction with outside world and within the cluster. API server validates and configures 
data for the API objects like Pods m Deployments, DaemonSets, Configmaps, ReplicationControllers. The are also responsible for Authentication.

#### [Kube Scheduler](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-scheduler/#:~:text=The%20Kubernetes%20scheduler%20is%20a,Pod%20to%20a%20suitable%20Node.)
Scheduler are responsible for assigning [Pods]() to Nodes. Scheduler knows all the resource that are available in other Nodes. The scheduler ranks or prioritize each node and match the Pods to the suitable Nodes. The scheduler is continually watching the state of the all Nodes.

#### [ETCD (Storage)]()
It is a form of Database in the Kubernetes Cluster. It is used to manage and store data that helps the distributed system running. It is highly-available key value store as kubernetes' backing store for all cluster Data. What does highly-available mean? That means there's no single point failure in the etcd cluster. It can resist the network partition and hardware failures. It is the core component of kubernetes is stores data and manages kubernetes `state data`, `configuration data`, and `metadata`. etcd relied is the single source of truth at a given point of time. Every node in etch cluster have access to the full data store. It is reliably consistent, every data reads in an etch cluster is going to return the most recent data.

* etcd is built on [Raft Algorithm]() that is used for distributed consensus.

* etcd is also `fast`, it is bench marked ar 10,000 writes per second. It does persist the data to the disk, etch performance is tied to our storage disk speed.

* `Secure`, etch use transport layer security with SSL authentication. it stores vital and highly sensitive configuration data.

* `Simple`, a web app can read and write data to etcd with simple http JSON tools.

#### [Kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/)
It is the primary node agent that runs on each node. It is responsible for managing container runtime. Based on that runtime the kubelet deploy pods. The kubelet registers the node with the apiserver using on of: the hostname; a flag to override the hostname; pr specific logic for a cloud provider.

THe kubelet works in terms of a PodSpec. A PodSpec is a YAML or JSON object that describes a pod.

#### [Kube Proxy (Node Network)]()

kube Proxy is responsible for managing the network connectivity of each container of the pods. All node within the cluster must have connectivity. 
