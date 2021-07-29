# Minikube
**Minikube** is a single-node cluster, but easy to work with kubernetes try stuff locally.

<img src="https://3.bp.blogspot.com/-lpJa8w6XMKY/XFhoY5dDQ8I/AAAAAAAADC0/t2Ojfo2CM_wbe1TGNo8jq5JE6JJYJbYbQCLcBGAs/s640/minikube-architecture.png" alt="minikube Architecture"/>

<br/>
Creating a cluster using `minikube` is easier. We just need to execute the below commands. if you are using any sort virtualization like docker, or VMs

for example, i'm using docker in my case
- The VM will get configured with Docker and Kubernetes via a single binary called ***localkube***.
```shell
minikube start --driver=docker

// or

minikube start --vm-driver=virtualbox
```
if you are using any other driver or you don't know what driver you are using
```
minikube start
```
- `minikube` will automatically recognize the virtual env.
- when we execute the `minikube start` command, it creates a new VM based on `minikube ` image.

```shell
➜  k8s-specs git:(master) ✗ minikube status
minikube
type: Control Plane 👈 // The master node responsible for scheduling
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

Minikube is running, and it initialized a Kubernetes cluster. It even configured
`kubectl` so that it points to the newly created VM.

Will open up the kubernetes dashboard
```
minikube dashboard
```