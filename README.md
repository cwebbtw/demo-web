# A web front end for demonstrating a simple web page using knative

## Prerequisites

* Docker
* Kubernetes context
* knative installed on the kubernetes cluster

## Releases

There is a blue release and a green release, which can be used to demonstrate deployment strategies.

Both releases have been deployed to dockerhub.

## knative

Using knative it is possible to show both versions of the web page being deployed using a canary deployment.

### Deploying blue

Deploying the blue release is done using `kubectl` to apply the deployment manifest for the blue release. Try:

`kubectl apply -f knative/demo-web-blue.yaml`

This manifest references the blue release which has been pushed to docker hub

### Deploying green

Deploying the green release is also done using `kubectl`. Apply the manifest in the same way using:

`kubectl apply -f knative/demo-web-green.yaml`

This manifest references the green release which has also been pushed to dockerhub. The slight difference in this deployment manifest is that the green release is deployed using the canary deployment strategy. Only 20% of the traffic will be hitting the green release.
