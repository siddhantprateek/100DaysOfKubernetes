### Day 9

# Kubernetes Service


```shell
git clone https://github.com/vfarcic/k8s-specs

git cd k8s-specs

kubectl config current-context

```
### Creating ReplicaSet


```shell

$ kubectl create -f svc/go-demo-2-rs.yml

replicaset.apps/go-demo-2 created
```

to know the state of the replicas 
```shell
$ kubectl get -f svc/go-demo-2-rs.yml
or
$ kubeclt get svc

NAME        DESIRED   CURRENT   READY   AGE
go-demo-2   2         2         2       3m3s
```
> We can see that the desired number of replicas is 2 and that it matches the current
value. The value of the ready field is still 0 but, after the images are pulled, and
the containers are running, it'll change to 2.







Instead of retrieving all the replicas in the cluster, we can retrieve those specified
in the rs/go-demo-2.yml file.
```shell
kubectl get -f svc/go-demo-2-rs.yml
```


With the kubectl expose command, we can tell Kubernetes that we want to expose a resource as service in our cluster.
```shell

// there are 4 different ways of exposing.

$ kubeclt expose rs go-demo-2 👈
or 
$ kubeclt expose --name=go-demo-2-svc 👈
or
// port: 28017 is mongodb port
$ kubeclt expose --target-port=28017 👈
or
$ kubeclt expose --type=NodePort 👈


service/go-demo-2 exposed
```


By Default we have three different types of services

**ClusterIP**

ClusterIP is used by default. It exposes the service only within the cluster. By default you want to be using ClusterIP since that prevents any external communication and makes your cluster more secure. 

**NodePort** 

Allows the outside world to access the node IP

**LoadBalancer**

The LoadBalancer is only useful when it is combined with the LoadBalancer of your cloud provider.

Services that newly created service have
```shell
$ kubectl describe -f svc/go-demo-2-rs.yml

Name:         go-demo-2
Namespace:    default
Selector:     service=go-demo-2,type=backend
Labels:       <none>
Annotations:  <none>
Replicas:     2 current / 2 desired
Pods Status:  2 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  db=mongo
           language=go
           service=go-demo-2
           type=backend
  Containers:
   db:
    Image:      mongo:3.3
    Port:       28017/TCP
    Host Port:  0/TCP
    Command:
      mongod
    Args:
      --rest
      --httpinterface
    Environment:  <none>
    Mounts:       <none>
   api:
    Image:      vfarcic/go-demo-2
    Port:       <none>
    Host Port:  <none>
    Liveness:   http-get http://:8080/demo/hello delay=0s timeout=1s period=10s #success=1 #failure=3
    Environment:
      DB:    localhost
    Mounts:  <none>
  Volumes:   <none>
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  27m   replicaset-controller  Created pod: go-demo-2-qxg8c
  Normal  SuccessfulCreate  27m   replicaset-controller  Created pod: go-demo-2-jg4g8

```

- All the pods in the cluster can access the targetPort
- The NodePort automatically creates the clusterIP
- Note that if you have multiple ports defined within a service, you have to name those ports


```
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  27m   replicaset-controller  Created pod: go-demo-2-qxg8c
  Normal  SuccessfulCreate  27m   replicaset-controller  Created pod: go-demo-2-jg4g8
```
we can see that ReplicaSet created two Pods while trying to match the desired state with the actual state.

```shell
$ kubectl get pods --show-labels

NAME              READY   STATUS    RESTARTS   AGE   LABELS
go-demo-2-jg4g8   2/2     Running   0          31m   db=mongo,language=go,service=go-demo-2,type=backend
go-demo-2-qxg8c   2/2     Running   0          31m   db=mongo,language=go,service=go-demo-2,type=backend
```
we used the `--show-labels` argument so that we can verify that the Pods in the cluster match those created by the ReplicaSet.

The sequence of events that transpired with the `kubectl create -f svc/go-demo-2-rs.yml` command is as follows:

- Kubernetes client (`kubectl`) sent a request to the API server requesting the creation of a ReplicaSet defined in the `rs/go-demo-2.yml` file.
- The controller is watching the API server for new events, and it detected that there is a new ReplicaSet object.
- The controller creates two new pod definitions because we have configured replica value as 2 in `rs/go-demo-2.yml` file.
- Since the scheduler is watching the API server for new events, it detected that there are two unassigned Pods.
- The scheduler decided to which node to assign the Pod and sent that information to the API server.
- Kubelet is also watching the API server. It detected that the two Pods were assigned to the node it is running on.
- Kubelet sent requests to Docker requesting the creation of the containers that form the Pod. In our case, the Pod defines two containers based on the `mongo` and `api` image. So in total four containers are created.
- Finally, Kubelet sent a request to the API server notifying it that the Pods were created successfully.


We can look at our endpoint through
```shell
$ kubectl get ep go-demo-2 -o yaml

apiVersion: v1
kind: Endpoints
metadata:
  annotations:
    endpoints.kubernetes.io/last-change-trigger-time: "2021-07-10T04:52:33Z"
  creationTimestamp: "2021-07-10T04:52:33Z"
  name: go-demo-2
  namespace: default
  resourceVersion: "1025348"
  uid: 7910373d-ae93-4b0c-b1f7-bf9fb0255101
subsets:
- addresses:
  - ip: 10.1.1.106
    nodeName: docker-desktop
    targetRef:
      kind: Pod
      name: go-demo-2-jg4g8
      namespace: default
      resourceVersion: "1024978"
      uid: 83053824-9f69-4b96-843c-fc0dc2b3abf0
  - ip: 10.1.1.107
    nodeName: docker-desktop
    targetRef:
      kind: Pod
      name: go-demo-2-qxg8c
      namespace: default
      resourceVersion: "1024992"
      uid: 9f9fd727-9815-489c-bc39-fc88eb827d63
  ports:
  - port: 28017
    protocol: TCP
```

Make sure to delete the Service and ReplicaSet at the end