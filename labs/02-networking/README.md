# Lab 2: Networking

Manifests for [Lab 2](https://workshop.platformfix.com/k8s/labs/2-networking/).

## What the lab applies

| File | What it is |
|------|------------|
| `backend-deployment.yaml` | The backend you first call by Pod IP, then by name. |
| `backend-service.yaml` | The ClusterIP Service that gives those Pods one stable address. |

## Extra, not used in the lab

Left in on purpose. Nothing in the lab applies these. Try them on your own cluster.

| File | What it demonstrates |
|------|---------------------|
| `frontend-deployment.yaml` | A second tier that reaches the backend by service name, set through an environment variable. |
| `frontend-service.yaml` | A ClusterIP Service in front of that frontend. |
| `backend-headless-service.yaml` | A headless Service, `clusterIP: None`. DNS returns the Pod IPs instead of one service IP. This is how StatefulSets address individual Pods. |
| `backend-loadbalancer-service.yaml` | External access with `type: LoadBalancer`. |
| `multi-label-deployment.yaml` | Version v1 of an app, labelled `app`, `tier`, and `version`. |
| `multi-label-deployment-v2.yaml` | Version v2 of the same app, running alongside v1. |
| `multi-label-service.yaml` | A Service whose selector matches v1 only. |
| `multi-label-service-v2.yaml` | The same, matching v2 only. |

Apply the two frontend files on top of the backend to watch one tier find another by name.

Apply all four multi-label files together to see how a selector picks one version out of two running side by side. That is how a blue/green release works. You shift traffic by editing a selector rather than redeploying.

`backend-loadbalancer-service.yaml` needs a cluster that can provision a load balancer. On kind or Docker Desktop the Service sits in `Pending` with no external IP, and you have lost nothing.

On a cloud you get the opposite problem: it works, and it costs money. The cluster used during the workshop runs on Civo, which has no ServiceLB. A `LoadBalancer` Service there is handled by `civo-ccm`, which creates a real Civo load balancer and charges for it. Nobody has measured what one costs, so treat the cost as real rather than assuming it is small.

Apply it deliberately if you want to see it, then `kubectl delete svc backend-loadbalancer` when you are done, and check your provider's console rather than assuming the bill stopped.
