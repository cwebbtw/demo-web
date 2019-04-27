# Canary deploy with knative

This demo-web repository provides the docker file, sample html and knative deployment manifests to canary deploy to a kubernetes cluster running knative.

## Prerequisites

* Docker
* Kubernetes context (AKS/EKS/GKE/minikube are all supported)
* knative (0.4) installed on the kubernetes cluster (knative works on minikube/AKS/EKS/etc)

See https://knative.dev/docs/install/ for information on installing knative.

## Releases

There is a blue release and a green release, which can be used to demonstrate deployment strategies.

Both releases have been deployed to dockerhub and are public.

See:

* `cwebb/demo-web:blue`
* `cwebb/demo-web:green`

## knative

Using knative it is possible to show both versions of the web page being deployed using a canary deployment.

### Deploying blue

Deploying the blue release is done using `kubectl` to apply the deployment manifest for the blue release. Try:

`kubectl apply -f knative/demo-web-blue.yaml`

This manifest references the blue release which has been pushed to docker hub

To access the deployment, use the following `kubectl` commands:

##### Obtaining the load balancer IP

`kubectl get service istio-ingressgateway -n istio-system`

Extract the external IP from the returned line

##### Obtaining the domain

`kubectl get route`

```
cwebb@CMP ~/Development/personal/demo-web [master ≡]$ kubectl get route
NAME       DOMAIN                                   READY   REASON
demo-web   demo-web.default.example.com   True
```

##### Putting them together

Hit the end point ensuring to pass the domain as the host header

`curl -H 'Host: <domain>' <IP>`

##### Further

You can edit the config domain in the knative-serving namespace to use a domain and override the need to specify a host header.

`kubectl edit cm config-domain -n knative-serving`

### Deploying green

Deploying the green release is also done using `kubectl`. Apply the manifest in the same way using:

`kubectl apply -f knative/demo-web-green.yaml`

This manifest references the green release which has also been pushed to dockerhub. The slight difference in this deployment manifest is that the green release is deployed using the canary deployment strategy. Only 20% of the traffic will be hitting the green release.
