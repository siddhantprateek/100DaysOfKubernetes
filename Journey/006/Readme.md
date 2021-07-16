# Running Pods


fork the repository from `https://github.com/vfarcic/k8s-specs.git`

```shell

git clone "https://github.com/vfarcic/k8s-specs.git"
```

after cloning the repository and hope into the directory,
```shell

cd k8s-specs
```

make sure that you have `kubectl` integrated with your terminal.

Creating a mangoDB pod.
```shell

kubectl run db --image mongo \ --generator "run-pod/v1"
```
or we can go for Nginx pod
```shell

kubectl run nginx --image=nginx
```

<img src="https://carbon.now.sh/embed?bg=rgba%2841%2C129%2C203%2C1%29&t=one-dark&wt=none&l=application%2Fx-sh&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=false&pv=67px&ph=42px&ln=false&fl=1&fm=Hack&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=%25E2%259E%259C%2520%2520k8s-specs%2520git%253A%28master%29%2520kubectl%2520run%2520db%2520--image%2520mongo%2520%255C%2520--generator%2520%2522run-pod%252Fv1%2522%250Apod%252Fdb%2520created%" alt=""/>

you will find something like this on your terminal.
```shell

pod/db created
```
To check whether the pod is up and running. run `kubectl get pods` in the terminal.


since the mongoDB pod is quiet big it might take a while to show the pod is running
```shell
➜  k8s-specs git:(master) kubectl get pods
NAME    READY   STATUS             RESTARTS   AGE
nginx   1/1     Running            0          11m
db      0/1     CrashLoopBackOff   9          24m
```
it takes time for mangoDB pod be ready.

To look into the pod definition
```shell
➜  k8s-specs git:(master) cat pod/db.yml

apiVersion: v1  <- version 1 of the Kubernetes pod API; API version and kind has to be provided -- it is mandatory
kind: Pod
metadata:       <- the metadata provides information on the pod, it does not specifiy how the pod behaves
name: db
  name: db
  labels:
    type: db
    vendor: MongoLabs     <- who has created the image
spec:
  containers:
  - name: db
    image: mongo:3.3       <- image name and tag
    command: ["mongod"]
    args: ["--rest", "--httpinterface"]  <- arguments, defined in an array

```

to delete a pod 
```shell

kubectl delete pod db
```

to view pod in json format
```shell

kubectl get pods -o json
```
JSON output
```
$ kubectl get pods -o json
{
    "apiVersion": "v1",
    "items": [
        {
            "apiVersion": "v1",
            "kind": "Pod",
            "metadata": {
                "annotations": {
                    "cni.projectcalico.org/podIP": "10.1.86.205/32",
                    "cni.projectcalico.org/podIPs": "10.1.86.205/32"
                },
                "creationTimestamp": "2021-06-09T06:30:42Z",
                "labels": {
                    "run": "nginx"
                },
                "name": "nginx",
                "namespace": "default",
                "resourceVersion": "43269",
                "selfLink": "/api/v1/namespaces/default/pods/nginx",
                "uid": "25956c79-c75e-492e-bf61-b3c72f08885f"
            },
            "spec": {
                "containers": [
                    {
                        "image": "nginx",
                        "imagePullPolicy": "Always",
                        "name": "nginx",
                        "resources": {},
                        "terminationMessagePath": "/dev/termination-log",
                        "terminationMessagePolicy": "File",
                        "volumeMounts": [
                            {
                                "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount",
                                "name": "default-token-wbv5d",
                                "readOnly": true
                            }
                        ]
                    }
                ],
                "dnsPolicy": "ClusterFirst",
                "enableServiceLinks": true,
                "nodeName": "codevalley",
                "preemptionPolicy": "PreemptLowerPriority",
                "priority": 0,
                "restartPolicy": "Always",
                "schedulerName": "default-scheduler",
                "securityContext": {},
                "serviceAccount": "default",
                "serviceAccountName": "default",
                "terminationGracePeriodSeconds": 30,
                "tolerations": [
                    {
                        "effect": "NoExecute",
                        "key": "node.kubernetes.io/not-ready",
                        "operator": "Exists",
                        "tolerationSeconds": 300
                    },
                    {
                        "effect": "NoExecute",
                        "key": "node.kubernetes.io/unreachable",
                        "operator": "Exists",
                        "tolerationSeconds": 300
                    }
                ],
                "volumes": [
                    {
                        "name": "default-token-wbv5d",
                        "secret": {
                            "defaultMode": 420,
                            "secretName": "default-token-wbv5d"
                        }
                    }
                ]
            },
            "status": {
                "conditions": [
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2021-06-09T06:30:42Z",
                        "status": "True",
                        "type": "Initialized"
                    },
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2021-06-09T06:30:51Z",
                        "status": "True",
                        "type": "Ready"
                    },
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2021-06-09T06:30:51Z",
                        "status": "True",
                        "type": "ContainersReady"
                    },
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2021-06-09T06:30:42Z",
                        "status": "True",
                        "type": "PodScheduled"
                    }
                ],
                "containerStatuses": [
                    {
                        "containerID": "containerd://eb5f2b854b58e8eca8c69d451b0665319e18965786d8ca89ad0b4fe7ada85e9c",
                        "image": "docker.io/library/nginx:latest",
                        "imageID": "docker.io/library/nginx@sha256:6d75c99af15565a301e48297fa2d121e15d80ad526f8369c526324f0f7ccb750",
                        "lastState": {},
                        "name": "nginx",
                        "ready": true,
                        "restartCount": 0,
                        "started": true,
                        "state": {
                            "running": {
                                "startedAt": "2021-06-09T06:30:50Z"
                            }
                        }
                    }
                ],
                "hostIP": "10.0.2.15",
                "phase": "Running",
                "podIP": "10.1.86.205",
                "podIPs": [
                    {
                        "ip": "10.1.86.205"
                    }
                ],
                "qosClass": "BestEffort",
                "startTime": "2021-06-09T06:30:42Z"
            }
        },
        {
            "apiVersion": "v1",
            "kind": "Pod",
            "metadata": {
                "annotations": {
                    "cni.projectcalico.org/podIP": "10.1.86.204/32",
                    "cni.projectcalico.org/podIPs": "10.1.86.204/32"
                },
                "creationTimestamp": "2021-06-09T06:17:34Z",
                "labels": {
                    "run": "db"
                },
                "name": "db",
                "namespace": "default",
                "resourceVersion": "45832",
                "selfLink": "/api/v1/namespaces/default/pods/db",
                "uid": "d4de4c42-1762-47e1-ad15-7296302865c8"
            },
            "spec": {
                "containers": [
                    {
                        "args": [
                            " --generator",
                            "run-pod/v1"
                        ],
                        "image": "mongo",
                        "imagePullPolicy": "Always",
                        "name": "db",
                        "resources": {},
                        "terminationMessagePath": "/dev/termination-log",
                        "terminationMessagePolicy": "File",
                        "volumeMounts": [
                            {
                                "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount",
                                "name": "default-token-wbv5d",
                                "readOnly": true
                            }
                        ]
                    }
                ],
                "dnsPolicy": "ClusterFirst",
                "enableServiceLinks": true,
                "nodeName": "codevalley",
                "preemptionPolicy": "PreemptLowerPriority",
                "priority": 0,
                "restartPolicy": "Always",
                "schedulerName": "default-scheduler",
                "securityContext": {},
                "serviceAccount": "default",
                "serviceAccountName": "default",
                "terminationGracePeriodSeconds": 30,
                "tolerations": [
                    {
                        "effect": "NoExecute",
                        "key": "node.kubernetes.io/not-ready",
                        "operator": "Exists",
                        "tolerationSeconds": 300
                    },
                    {
                        "effect": "NoExecute",
                        "key": "node.kubernetes.io/unreachable",
                        "operator": "Exists",
                        "tolerationSeconds": 300
                    }
                ],
                "volumes": [
                    {
                        "name": "default-token-wbv5d",
                        "secret": {
                            "defaultMode": 420,
                            "secretName": "default-token-wbv5d"
                        }
                    }
                ]
            },
            "status": {
                "conditions": [
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2021-06-09T06:17:34Z",
                        "status": "True",
                        "type": "Initialized"
                    },
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2021-06-09T07:06:15Z",
                        "message": "containers with unready status: [db]",
                        "reason": "ContainersNotReady",
                        "status": "False",
                        "type": "Ready"
                    },
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2021-06-09T07:06:15Z",
                        "message": "containers with unready status: [db]",
                        "reason": "ContainersNotReady",
                        "status": "False",
                        "type": "ContainersReady"
                    },
                    {
                        "lastProbeTime": null,
                        "lastTransitionTime": "2021-06-09T06:17:34Z",
                        "status": "True",
                        "type": "PodScheduled"
                    }
                ],
                "containerStatuses": [
                    {
                        "containerID": "containerd://2c21fb861d32c61f213b03914528355605f45fb4f9abf98770cb02b5a60c1aa8",
                        "image": "docker.io/library/mongo:latest",
                        "imageID": "docker.io/library/mongo@sha256:8b35c0a75c2dbf23110ed2485feca567ec9ab743feee7a0d7a148f806daf5e86",
                        "lastState": {
                            "terminated": {
                                "containerID": "containerd://2c21fb861d32c61f213b03914528355605f45fb4f9abf98770cb02b5a60c1aa8",
                                "exitCode": 127,
                                "finishedAt": "2021-06-09T07:06:14Z",
                                "reason": "Error",
                                "startedAt": "2021-06-09T07:06:14Z"
                            }
                        },
                        "name": "db",
                        "ready": false,
                        "restartCount": 14,
                        "started": false,
                        "state": {
                            "waiting": {
                                "message": "back-off 5m0s restarting failed container=db pod=db_default(d4de4c42-1762-47e1-ad15-7296302865c8)",
                                "reason": "CrashLoopBackOff"
                            }
                        }
                    }
                ],
                "hostIP": "10.0.2.15",
                "phase": "Running",
                "podIP": "10.1.86.204",
                "podIPs": [
                    {
                        "ip": "10.1.86.204"
                    }
                ],
                "qosClass": "BestEffort",
                "startTime": "2021-06-09T06:17:34Z"
            }
        }
    ],
    "kind": "List",
    "metadata": {
        "resourceVersion": "",
        "selfLink": ""
    }
}

```

to verify database is running or not
```
kubectl exec -it db sh 
```

if we are not using the pod we must delete it.
```shell
kubeclt delete -f pod/db.yml
```