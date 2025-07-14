# otel-k8s-springboot

A complete Helm-based deployment example for end-to-end OpenTelemetry observability on Kubernetes, including multiple Spring Boot microservices and PostgreSQL.

## What’s Included

* OpenTelemetry Operator and Java auto-instrumentation
* OTEL Collector (with logging + Jaeger exporters)
* Jaeger for distributed trace visualization
* Spring Boot Microservices: `order-service` and `payment-service`
* PostgreSQL backend

---

## Prerequisites

* [Install cert-manager on Kubernetes](cert-manager-install.md)
* Minikube installed
* Docker and kubectl installed
* Helm installed

---

## Kubernetes Namespace Layout

| Component              | Namespace       |
| ---------------------- | --------------- |
| OTEL Collector         | `observability` |
| Jaeger                 | `observability` |
| OpenTelemetry Operator | `observability` |
| Spring Boot apps       | `demo-apps`     |
| Postgres database      | `demo-apps`     |
| Instrumentation CRD    | `demo-apps`     |

---

## Setup Instructions

```bash
# Clone repository
git clone <repo-url>
cd otel-k8s-springboot

# Start Minikube
minikube start --cpus=4 --memory=4g

# Create namespaces
kubectl create namespace observability
kubectl create namespace demo-apps

# Install OpenTelemetry Operator
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm install otel-operator open-telemetry/opentelemetry-operator \
  --namespace observability

# Install OTEL Collector and Jaeger (observability namespace)
helm install otel-stack ./charts/otel-stack -n observability

# (Optional) Install Jaeger separately
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
helm repo update
helm upgrade --install jaeger jaegertracing/jaeger \
  --namespace observability \
  --set storage.type=memory \
  --set collector.enabled=true \
  --set agent.enabled=true \
  --set query.enabled=true


# Build Spring Boot images
cd src
mvn package
# Build order-service
docker build -t order-service:0.0.1-SNAPSHOT ./order-service
# Build payment-service
docker build -t payment-service:0.0.1-SNAPSHOT ./payment-service
cd ..

# Install Spring Boot app stack (demo-apps namespace)
helm install app-stack ./charts/app-stack -n demo-apps

# Install Otel stack for the collector and Jaeger(observability namespace)
helm install otel-stack ./charts/otel-stack -n observability

# Apply instrumentation configuration
kubectl apply -f instrumentation.yaml -n demo-apps
```

---

## Verify Deployment

```bash
kubectl get pods -n observability          # Check OTEL Collector and Jaeger
kubectl get pods -n demo-apps              # Check Spring Boot apps and Postgres

minikube service order-service -n demo-apps --url &
minikube service payment-service -n demo-apps --url &

kubectl logs deploy/otel-collector -n observability
```

## Access Jaeger UI

```shell
     minikube service jaeger-query -n observability --url
```
  ### via Port forwarding 
```shell 
    export POD_NAME=$(kubectl get pods --namespace observability -l "app.kubernetes.io/instance=jaeger,app.kubernetes.io/component=query" -o jsonpath="{.items[0].metadata.name}")
```
```shell  
   kubectl port-forward --namespace observability $POD_NAME 8080:16686
```
---


## Extending and Customizing

* Add Prometheus exporter to OTEL Collector for metrics
* Add Grafana dashboards
* Add Loki for logs aggregation
* Deploy in multi-node Kubernetes clusters

---

## Notes

* The Instrumentation CRD must be in the same namespace as the applications (`demo-apps`).
* The OTEL Collector and Jaeger must run in the `observability` namespace.
* All microservices are configured to send telemetry to the OTEL Collector.
* This setup uses separate Helm charts for the observability stack (`otel-stack`) and the application stack (`app-stack`).
