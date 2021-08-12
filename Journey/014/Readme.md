# Kubernetes Volumes



We cannot store data within a container. The problem is that when the container crashes and the kubelet restarts the container but the state of the container is lost. Kubernetes does not give data persistance out of the box.

**Volumes are references of files and directories** mad accessible to containers that form a pod. So the Volume keeps the track of the state of the application and if one pod dies the next pod will have access to the volumes and thus, the previously recorded state.

> 💡 There are over 25 different Volume types within Kubernetes - some of which are specific to hosting providers. i.e.AWS

The difference between volumes is the way that files and  directories are created.
- Volumes can also be used to access other Kubernetes resources such as to access the Docker socket.
- Kubernetes Volumes have to be highly error-resistant — and even survive a crash of the entire cluster.
- Volumes and Persistent Volumes are created like other Kubernetes resources, through YAML files.

Additionally, we can differentiate between **remote and local volumes** — each volume type has its own use case. 

Local volumes are tied to a specific node and do not survive cluster disasters. Thus, you want to use remote volumes whenever possible.


```

apiVersion: v1
kind: Pod
metadata:
  name: empty-dir
spec:
  containers:
    - name: busybox-a
      command: ['tail', '-f', '/dev/null']
      image: busybox
      volumeMounts:
        - name: cache
          mountPath: /cache
    - name: busybox-b
      command: ['tail', '-f', '/dev/null']
      image: busybox
      volumeMounts:
        - name: cache
          mountPath: /cache
  volumes:
    - name: cache
      emptyDir: {}

```

- Create a resource 

```
kubectl apply -f empty-dir
```

- Write to file 

```
kubectl exec empty-dir --container busybox-a -- sh -c "echo \"Hello World\" > /cache/hello.txt"
```

- Read what is within the file

```
kubectl exec empty-dir --container busybox-b -- cat /cache/hello.txt
```

To ensure that the data will be saved beyond the creation and deletion of pods, we need Persistent volumes. Ephemeral volume types only have the lifetime of a pod — thus, they are not of much use if the pod crashes.

A persistent volume will have to take the same storage as the physical storage.

A hostPath volume mounts a file or directory from the host node’s filesystem into your Pod. This is not something that most Pods will need, but it offers a powerful escape hatch for some applications.

```
apiVersion: v1
kind: Pod
metadata:
  name: host-path
spec:
  containers:
    - name: busybox
      command: ['tail', '-f', '/dev/null']
      image: busybox
      volumeMounts:
        - name: data
          mountPath: /data
  volumes:
  - name: data
    hostPath:
      path: /data
```

- Create the volume
```
kubectl apply -f empty-dir
```
- Read from the volume
```
kubectl exec host-path -- cat /data/hello.txt
```

**Resource**: 
<a href="https://www.youtube.com/watch?v=0swOh5C3OVM" target="_blank"> <img src="https://i.imgur.com/zTPnWyu.jpg" target="_blank"/></a>
