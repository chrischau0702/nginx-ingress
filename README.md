# Using NGINX Ingress in GKE

Guide to deploy Nginx as Ingressin Google's Managed Kubernetes Service (GKE)

### Pre-requisites:

1. Existing GKE Kubernetes cluster
2. Google Cloud Shell with necessary permissions

## Create namespaces and other dependencies for Nginx Ingress:

```
kubectl create -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/mandatory.yaml
```

## Create the Service for Nginx Ingress controller:

Executing the below command would create a Google Load Balancer to receive incoming traffic for your microservice.

```
kubectl create -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/provider/cloud-generic.yaml
```

## Deploying a Microservice:

```
kubectl create -f https://raw.githubusercontent.com/christus02/nginx-ingress/master/manifests/apache.yaml
```

## Create Ingress rules for Microservice:

```
kubectl create -f https://raw.githubusercontent.com/christus02/nginx-ingress/master/manifests/ingress-apache.yaml
```


## Send traffic to the Microservice:

First capture the External IP (Google Load-balancer's IP) from Kubernetes service.

**note** that the service, deployments and Ingress would be created in a separate namespace - **"ingress-nginx"**

```
$ kubectl get svc -n ingress-nginx
NAME            TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
apache          ClusterIP      10.3.250.48    <none>          80/TCP                       1h
ingress-nginx   LoadBalancer   10.3.243.184   1.1.1.1         80:32309/TCP,443:31980/TCP   1h
```

**NOTE:**
The external IP from the previous output has been replaced with dummy IP (1.1.1.1) for obvious reasons.

Now send a curl to the External IP with Hostname to access the Microservice

```
$ curl http://1.1.1.1/ -H 'Host: nginx-ingress-gke.com'
<html><body><h1>It works!</h1></body></html>
```

The curl response is the response from the actual microservice (apache) inside the Kubernetes cluster. 
