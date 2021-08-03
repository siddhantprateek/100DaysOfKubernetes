### Day 13

# [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)

**Ingress** is an API object responsible for managing the external access to our cluster, typically HTTP.

Another defination can be, Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

 Whereby it manages
- Forwarding rules based on paths and domains
- SSL/TLS termination
- Load Balancing 
- name-based virtual hosting
- and Several other features.

Things we want to resolve using Ingress

- Not having to use a fixed port — if we have to manage multiple clusters, we would have a hard time managing all those ports
- We need standard HTTPS(443) or HTTP (80) ports through a predefined path

When we open an application, the request to the application is first received by the service and LoadBalancer, which is the responsible for forwarding the request to either of the pods it is responsible for.

To make our application more secure, we need a place to store the application's HTTPS certificate and forwarding. Once this is implemented, we have a mechanism that accepts requests on specific ports and forwards them to our Kubernetes Service.

The Ingress Controller can be used for this. 

Unlike other Kubernetes Controllers, it is not part of our cluster by default but we have to install it separately.

If you are using minikube, you can check the available addons through
```

minikube addons list
```
```
|-----------------------------|----------|--------------|-----------------------|
|         ADDON NAME          | PROFILE  |    STATUS    |      MAINTAINER       |
|-----------------------------|----------|--------------|-----------------------|
| ambassador                  | minikube | disabled     | unknown (third-party) |
| auto-pause                  | minikube | disabled     | google                |
| csi-hostpath-driver         | minikube | disabled     | kubernetes            |
| dashboard                   | minikube | enabled ✅   | kubernetes            |
| default-storageclass        | minikube | enabled ✅   | kubernetes            |
| efk                         | minikube | disabled     | unknown (third-party) |
| freshpod                    | minikube | disabled     | google                |
| gcp-auth                    | minikube | disabled     | google                |
| gvisor                      | minikube | disabled     | google                |
| helm-tiller                 | minikube | disabled     | unknown (third-party) |
| ingress                     | minikube | disabled     | unknown (third-party) |
| ingress-dns                 | minikube | disabled     | unknown (third-party) |
| istio                       | minikube | disabled     | unknown (third-party) |
| istio-provisioner           | minikube | disabled     | unknown (third-party) |
| kubevirt                    | minikube | disabled     | unknown (third-party) |
| logviewer                   | minikube | disabled     | google                |
| metallb                     | minikube | disabled     | unknown (third-party) |
| metrics-server              | minikube | disabled     | kubernetes            |
| nvidia-driver-installer     | minikube | disabled     | google                |
| nvidia-gpu-device-plugin    | minikube | disabled     | unknown (third-party) |
| olm                         | minikube | disabled     | unknown (third-party) |
| pod-security-policy         | minikube | disabled     | unknown (third-party) |
| registry                    | minikube | disabled     | google                |
| registry-aliases            | minikube | disabled     | unknown (third-party) |
| registry-creds              | minikube | disabled     | unknown (third-party) |
| storage-provisioner         | minikube | enabled ✅   | kubernetes            |
| storage-provisioner-gluster | minikube | disabled     | unknown (third-party) |
| volumesnapshots             | minikube | disabled     | kubernetes            |
|-----------------------------|----------|--------------|-----------------------|

```

And enable ingress (in case it is not enabled)
```
minikube addons enable ingress
```

If you are on microk8s, you can enable ingress through
```
microk8s enable ingress
```

You can check whether it is running through - if the pods for ingress-nginx , they will be listed
```
kubectl get pods --all-namespaces | grep ingress-nginx
```

to be continued...