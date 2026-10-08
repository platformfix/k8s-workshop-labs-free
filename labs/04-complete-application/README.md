# Lab 4: The Complete Application

Manifests for [Lab 4](https://workshop.platformfix.com/k8s/labs/4-complete-application/).

## What the lab applies

Every manifest in this folder is used. There is nothing extra here.

| File | What it is |
|------|------------|
| `app-configmap.yaml` | App settings and the nginx config the frontend serves from. |
| `mysql-data-pvc.yaml` | The claim the database keeps its data on. It names no storage class, so it binds to your cluster default. |
| `mysql-deployment.yaml` | MySQL, backed by that claim, plus the headless Service that gives it a name. |
| `backend-deployment.yaml` | The internal API tier, plus its ClusterIP Service. The lab autoscales the Deployment. |
| `frontend-content-configmap.yaml` | The page the frontend serves. |
| `frontend-deployment.yaml` | The frontend tier, mounting both ConfigMaps, plus the Service you port-forward to. |

Three of these files hold two objects each. The Deployment comes first, then the Service that fronts it, separated by `---`. One `kubectl apply -f` creates both.

That is why `kubectl get pods,svc -l app=frontend` shows a Service straight after you applied what looks like a Deployment file.

The lab also creates a Secret named `app-secrets` by hand, which `mysql-deployment.yaml` reads:

```bash
kubectl create secret generic app-secrets \
  --from-literal=db-password='change-me-in-real-life'
```

## Reaching the frontend

`frontend-service` is a ClusterIP, so it works the same on every cluster:

```bash
kubectl port-forward svc/frontend-service 8080:80
```

You can change the Service `type` to `LoadBalancer` and use the external address instead, but read this first if your cluster is a cloud one.

On kind or Docker Desktop that switch is free: nothing can provision a load balancer, so the Service sits in `Pending` and you have lost nothing. On a cloud it is not free. The cluster used during the workshop runs on Civo, which has no ServiceLB. A `LoadBalancer` Service there is handled by `civo-ccm`, which creates a real Civo load balancer and charges for it. Nobody has measured what one costs, so treat the cost as real rather than assuming it is small.

`kubectl delete svc frontend-service` removes it again, and it is worth checking your provider's console afterwards rather than assuming the bill stopped.

Port-forward needs none of that, which is why the lab uses it.

Deleting the namespace at the end removes all of it in one go.
