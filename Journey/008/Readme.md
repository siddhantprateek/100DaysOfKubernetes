### Day 8
# Kubernetes ReplicaSet
## ReplicaSets
A ReplicaSet creates desired number of pods using pod template it ensures certain number of pods are running at a particular time. If there are more pods running, the ReplicaSets will kill the pods. This is the reason pods are vulnereable one the pods gets deleted the containers too. 
### Why are we running multiple pods?0
To scale our application we need multiple pods running inside kuberenetes cluster.


The solution for this in kubernetes controller the which is a category of component which takes the reponsibility of rescheduling for a pod which goes down.

### ReplicaSet Components 
It is composed of two important parts 
* `Pod Template` : A template for new pods, and that gets the pod config file
* `Replica Count` :  The desired number of pods that a controller is supposed to keep running at all times.

As  we know when we create ReplicaSet it creates a desired number of pod using pod templates if any of those pods die or get evicted the ReplicaSet creates a replacement, if ReplicaSets discover a  extra pod running then it will randomly delete of the pod.



```shell
cat svc/go-demo-2-rs.yml
```
```shell
apiVersion:  apps/v1
kind: ReplicaSet
metadata:
  name: go-demo-2
spec:
  replicas: 2
  selector:
    matchLabels:
      type: backend
      service: go-demo-2
  template:
    metadata:
      labels:
        type: backend
        service: go-demo-2
        db: mongo
        language: go
    spec:
      containers:         // There are two containers running
      - name: db        
        image: mongo:3.3            👈
        command: ["mongod"]
        args: ["--rest", "--httpinterface"]
        ports:
        - containerPort: 28017
          protocol: TCP
      - name: api       
        image: vfarcic/go-demo-2    👈
        env:
        - name: DB
          value: localhost
        livenessProbe:
          httpGet:
            path: /demo/hello
            port: 8080

```


### Creating ReplicaSet

```shell

$ kubectl create -f svc/go-demo-2-rs.yml

replicaset.apps/go-demo-2 created
```

to know the state of the replicas 
```shell
$ kubectl get -f svc/go-demo-2-rs.yml


NAME        DESIRED   CURRENT   READY   AGE
go-demo-2   2         2         2       3m3s
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





