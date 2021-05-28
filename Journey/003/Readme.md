
# Day 3:
### Worker Node
![Screenshot 2021-05-27 155836](https://user-images.githubusercontent.com/43869046/119820370-07ba5a00-bf0f-11eb-9a45-195e3b09c8ea.png)
In Kubernetes cluster kubelet and kube Proxy in main node interact with the worker node. Each worker node consist of a Kube Proxy and kubelet.These worker nodes runs the containerized applications. The worker node(s) host the Pods that are the components of the application workload. 

#### Pods
In Kubernetes a pod is atomic unit of scheduling. It can contain one or more than one containers. 
Note: `Each and every pod in kubernetes cluster have a unique IP address.`
When we create a pod in the worker node it goes in the state called Pending. What does Pending means in this case? This meant to say that the kubernetes API have accepted the pod and ready to roll, but hasn't been schedule into a machine.

![Group 3](https://user-images.githubusercontent.com/43869046/119820415-143eb280-bf0f-11eb-9aae-bf6fb2219f46.png)

Let's see a case where the pod is assigned to node by the scheduler. The pod transition out from Pending state into the Creating state. In the Creating state the image need to be pulled the image is in the cloud based repository. 

If the image is already present in the node the pulling step will be skipped. After the image is been pulled the container transition into the running state.

