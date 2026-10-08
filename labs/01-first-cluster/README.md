# Lab 1: Your First Cluster

Manifests for [Lab 1](https://workshop.platformfix.com/k8s/labs/1-first-cluster/).

## What the lab applies

| File | What it is |
|------|------------|
| `deployment.yaml` | The nginx Deployment you deploy, scale, break, and roll back. |
| `broken-deployment.yaml` | The same Deployment with a command that exits 1. Apply it over the good one to trigger `CrashLoopBackOff`. |

Run `diff deployment.yaml broken-deployment.yaml` to see the whole of what breaks it.

## Extra, not used in the lab

Left in on purpose. Nothing in the lab applies these. Try them on your own cluster.

| File | What it demonstrates |
|------|---------------------|
| `nginx-pod.yaml` | A bare Pod, with no Deployment above it. Delete it and nothing brings it back. That gap is why you run Deployments. |
| `configmap.yaml` | Configuration held outside the image: app settings and an `index.html` page. |
| `webserver-deployment.yaml` | A Deployment that mounts that ConfigMap as its web content and reads environment variables from a Secret. |

`webserver-deployment.yaml` needs two things first. Apply `configmap.yaml`, then create a Secret named `app-secrets` holding `db-password` and `api-key`:

```bash
kubectl create secret generic app-secrets \
  --from-literal=db-password='change-me' \
  --from-literal=api-key='change-me'
```
