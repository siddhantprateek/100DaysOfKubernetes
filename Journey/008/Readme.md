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