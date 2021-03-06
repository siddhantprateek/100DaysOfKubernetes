# [Kubernetes Deployment](https://www.redhat.com/en/topics/containers/what-is-kubernetes-deployment)

[![Watch the video](https://i.ytimg.com/vi/EUitQ8DaZW8/maxresdefault.jpg)](https://www.youtube.com/embed/mNK14yXIZF4)

A _Deployment_ provides declarative updates for [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) and [ReplicaSets](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/).

> **Note:** Do not manage ReplicaSets owned by a Deployment.

A  [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes)  deployment is a resource object in Kubernetes that provides declarative updates to applications. A deployment allows you to describe an application’s life cycle, such as which images to use for the app, the number of pods there should be, and the way in which they should be updated.

A Kubernetes object is a way to tell the Kubernetes system how you want your cluster’s workload to look. After an object has been created, the cluster works to ensure that the object exists, maintaining the desired state of your  [Kubernetes cluster](https://www.redhat.com/en/topics/containers/what-is-a-kubernetes-cluster).

```yaml
// create deployment.yaml file


apiVersion: apps/v1
kind: Deployment
metadata:
  name: react-application  👈 your deployment name
spec:
  replicas: 2   👈 we alter the no. of replica want
  selector:
    matchLabels:
      run: react-application
  template:
    metadata:
      labels:
        run: react-application
    spec:
      containers:
      - name: react-application
        image: anaisurlichs/react-article-display:master
        ports:
          - containerPort: 80
        imagePullPolicy: Always

```

More information on Kubernetes deployments

-   A deployment is a Kubernetes object that makes it possible to manage multiple, identical pods
-   Using deployments, it is possible to automate the process of creating, modifying and deleting pods — it basically manages the lifecycle of your application
-   Whenever a new object is created, Kubernetes will ensure that this object exist
-   If you try to set-up pods manually, it can lead to human error, using deployments
-   The difference between a **deployment** and a **service** is that a deployment ensures that a set of pods keeps running by creating pods and replacing broken pods with the resource defined in the template. In comparison, a service is used to allow a network to access the running pods.


To create deployment
```shell
$ kubectl create -f deployment.yaml
```

To access the information on the deployement:
```
$ kubectl describe deployment <deployment-name> 
// deployment-name: react-application
```

Create `service.yaml`
```
$ kubectl expose deployment/my-nginx
```



To delete the deployment
```
kubectl delete deployment <deployment-name>
```
To delete the deployment
```
kubectl delete deployment <deployment-name>
```

Create Deployment from cloud
```
kubectl create deployment nginx-depl --image=nginx
```
will download latest nginx image from docker hub

```
kubectl get deployment
```
```
kubectl get pod
```
```
kubectl get replicaset
```

> **Note**: We do not interact directly with pod in kubernetes cluster. There are layers to which we interact. We interact with Deployment which is the abstraction of pod.
 * We Interact with deployment it interact with ReplicaSet that interact with pod.

### Debugging Pods

```
kubectl logs [pod-name]
```

for configuration:
creating the configuration yaml file for the deployment
```
kubectl apply -f nginx-deployment.yaml
```
```
touch nginx-deployment.yaml
```
