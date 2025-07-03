# Installing cert-manager on Kubernetes

This guide provides step-by-step instructions to install **cert-manager** on a Kubernetes cluster using Helm. The installation includes setting up the required Custom Resource Definitions (CRDs) and deploying cert-manager in its own namespace.

---

## Prerequisites

* A running Kubernetes cluster (minikube, kind, EKS, GKE, etc.)
* `kubectl` configured to access the cluster
* `helm` installed and configured

---

## Step 1: Install cert-manager CRDs

cert-manager requires several Custom Resource Definitions (CRDs) to function properly. Apply them before installing cert-manager via Helm:

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.15.0/cert-manager.crds.yaml
```

**Note:** This step is mandatory. Without these CRDs, Kubernetes will not recognize cert-manager resources such as `Certificate` and `Issuer`.

---

## Step 2: Add the Jetstack Helm repository

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

---

## Step 3: Install cert-manager using Helm

Create a dedicated namespace for cert-manager and install it using the latest stable chart:

```bash
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.15.0
```# Installing cert-manager on Kubernetes

This guide provides step-by-step instructions to install **cert-manager** on a Kubernetes cluster using Helm. The installation includes setting up the required Custom Resource Definitions (CRDs) and deploying cert-manager in its own namespace.

---

## Prerequisites

* A running Kubernetes cluster (minikube, kind, EKS, GKE, etc.)
* `kubectl` configured to access the cluster
* `helm` installed and configured

---

## Step 1: Install cert-manager CRDs

cert-manager requires several Custom Resource Definitions (CRDs) to function properly. Apply them before installing cert-manager via Helm:

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.15.0/cert-manager.crds.yaml
```

**Note:** This step is mandatory. Without these CRDs, Kubernetes will not recognize cert-manager resources such as `Certificate` and `Issuer`.

---

## Step 2: Add the Jetstack Helm repository

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

---

## Step 3: Install cert-manager using Helm

Create a dedicated namespace for cert-manager and install it using the latest stable chart:

```bash
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.15.0
```

**Best Practice:** Keep cert-manager in its own namespace (e.g., `cert-manager`) separate from application namespaces.

---

## Step 4: Verify cert-manager Installation

Check if the cert-manager pods are running:

```bash
kubectl get pods -n cert-manager
```

Expected output:

```
NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-xxxx                         1/1     Running   0          1m
cert-manager-cainjector-xxxx              1/1     Running   0          1m
cert-manager-webhook-xxxx                 1/1     Running   0          1m
```

Check CRDs to confirm successful installation:

```bash
kubectl get crd | grep cert-manager
```

You should see:

```
certificates.cert-manager.io
issuers.cert-manager.io
...
```

---

## Summary

| Component    | Namespace      | Purpose                              |
| ------------ | -------------- | ------------------------------------ |
| cert-manager | `cert-manager` | Manages TLS certificates and issuers |

cert-manager is now installed and ready to issue certificates in any namespace, including for applications like OpenTelemetry Operator, Ingress controllers, and more.

---

**Next Steps:**

* Deploy applications that use cert-manager to manage TLS (e.g., OpenTelemetry Operator)
* Create `Issuer` and `Certificate` resources as needed

---

For more information: [https://cert-manager.io/docs/](https://cert-manager.io/docs/)
# Installing cert-manager on Kubernetes

This guide provides step-by-step instructions to install **cert-manager** on a Kubernetes cluster using Helm. The installation includes setting up the required Custom Resource Definitions (CRDs) and deploying cert-manager in its own namespace.

---

## Prerequisites

* A running Kubernetes cluster (minikube, kind, EKS, GKE, etc.)
* `kubectl` configured to access the cluster
* `helm` installed and configured

---

## Step 1: Install cert-manager CRDs

cert-manager requires several Custom Resource Definitions (CRDs) to function properly. Apply them before installing cert-manager via Helm:

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.15.0/cert-manager.crds.yaml
```

**Note:** This step is mandatory. Without these CRDs, Kubernetes will not recognize cert-manager resources such as `Certificate` and `Issuer`.

---

## Step 2: Add the Jetstack Helm repository

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

---

## Step 3: Install cert-manager using Helm

Create a dedicated namespace for cert-manager and install it using the latest stable chart:

```bash
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.15.0
```

 **Best Practice:** Keep cert-manager in its own namespace (e.g., `cert-manager`) separate from application namespaces.

---

## Step 4: Verify cert-manager Installation

Check if the cert-manager pods are running:

```bash
kubectl get pods -n cert-manager
```

Expected output:

```
NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-xxxx                         1/1     Running   0          1m
cert-manager-cainjector-xxxx              1/1     Running   0          1m
cert-manager-webhook-xxxx                 1/1     Running   0          1m
```

Check CRDs to confirm successful installation:

```bash
kubectl get crd | grep cert-manager
```

You should see:

```
certificates.cert-manager.io
issuers.cert-manager.io
...
```

---

## Summary

| Component    | Namespace      | Purpose                              |
| ------------ | -------------- | ------------------------------------ |
| cert-manager | `cert-manager` | Manages TLS certificates and issuers |

cert-manager is now installed and ready to issue certificates in any namespace, including for applications like OpenTelemetry Operator, Ingress controllers, and more.

---

**Next Steps:**

* Deploy applications that use cert-manager to manage TLS (e.g., OpenTelemetry Operator)
* Create `Issuer` and `Certificate` resources as needed

---

For more information: [https://cert-manager.io/docs/](https://cert-manager.io/docs/)
# Installing cert-manager on Kubernetes

This guide provides step-by-step instructions to install **cert-manager** on a Kubernetes cluster using Helm. The installation includes setting up the required Custom Resource Definitions (CRDs) and deploying cert-manager in its own namespace.

---

## Prerequisites

* A running Kubernetes cluster (minikube, kind, EKS, GKE, etc.)
* `kubectl` configured to access the cluster
* `helm` installed and configured

---

## Step 1: Install cert-manager CRDs

cert-manager requires several Custom Resource Definitions (CRDs) to function properly. Apply them before installing cert-manager via Helm:

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.15.0/cert-manager.crds.yaml
```

**Note:** This step is mandatory. Without these CRDs, Kubernetes will not recognize cert-manager resources such as `Certificate` and `Issuer`.

---

## Step 2: Add the Jetstack Helm repository

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

---

## Step 3: Install cert-manager using Helm

Create a dedicated namespace for cert-manager and install it using the latest stable chart:

```bash
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.15.0
```

**Best Practice:** Keep cert-manager in its own namespace (e.g., `cert-manager`) separate from application namespaces.

---

## Step 4: Verify cert-manager Installation

Check if the cert-manager pods are running:

```bash
kubectl get pods -n cert-manager
```

Expected output:

```
NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-xxxx                         1/1     Running   0          1m
cert-manager-cainjector-xxxx              1/1     Running   0          1m
cert-manager-webhook-xxxx                 1/1     Running   0          1m
```

Check CRDs to confirm successful installation:

```bash
kubectl get crd | grep cert-manager
```

You should see:

```
certificates.cert-manager.io
issuers.cert-manager.io
...
```

---

## Summary

| Component    | Namespace      | Purpose                              |
| ------------ | -------------- | ------------------------------------ |
| cert-manager | `cert-manager` | Manages TLS certificates and issuers |

cert-manager is now installed and ready to issue certificates in any namespace, including for applications like OpenTelemetry Operator, Ingress controllers, and more.

---

**Next Steps:**

* Deploy applications that use cert-manager to manage TLS (e.g., OpenTelemetry Operator)
* Create `Issuer` and `Certificate` resources as needed

---

For more information: [https://cert-manager.io/docs/](https://cert-manager.io/docs/)


**Best Practice:** Keep cert-manager in its own namespace (e.g., `cert-manager`) separate from application namespaces.

---

## Step 4: Verify cert-manager Installation

Check if the cert-manager pods are running:

```bash
kubectl get pods -n cert-manager
```

Expected output:

```
NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager-xxxx                         1/1     Running   0          1m
cert-manager-cainjector-xxxx              1/1     Running   0          1m
cert-manager-webhook-xxxx                 1/1     Running   0          1m
```

Check CRDs to confirm successful installation:

```bash
kubectl get crd | grep cert-manager
```

You should see:

```
certificates.cert-manager.io
issuers.cert-manager.io
...
```

---

## Summary

| Component    | Namespace      | Purpose                              |
| ------------ | -------------- | ------------------------------------ |
| cert-manager | `cert-manager` | Manages TLS certificates and issuers |

cert-manager is now installed and ready to issue certificates in any namespace, including for applications like OpenTelemetry Operator, Ingress controllers, and more.

---

**Next Steps:**

* Deploy applications that use cert-manager to manage TLS (e.g., OpenTelemetry Operator)
* Create `Issuer` and `Certificate` resources as needed

---

For more information: [https://cert-manager.io/docs/](https://cert-manager.io/docs/)
