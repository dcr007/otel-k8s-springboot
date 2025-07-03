# otel-k8s-springboot

A sample Helm chart that deploys:

* OpenTelemetry Operator and Java auto-instrumentation
* OTEL Collector (logging mode)
* Spring Boot application (with OTEL auto-injection)

## Prerequisites

* [Install cert-manager on Kubernetes](cert-manager-install.md)
* Minikube or kind installed
* Docker and kubectl installed
* Helm installed

## Setup

```bash
git clone <repo-url>
cd otel-k8s-springboot

# Start local Kubernetes
minikube start --cpus=4 --memory=4g

# Install Operator (namespace "observability")
kubectl create namespace observability
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm install otel-operator open-telemetry/opentelemetry-operator \
  --namespace observability

# Build Spring Boot image (if needed)
cd src
mvn package
docker build -t springboot-app:latest .

cd ..
helm install telemetry . --namespace default
```

## Verify

```bash
kubectl get pods
minikube service springboot-app --url   # to generate trace
kubectl logs deploy/otel-collector     # view trace logs
```

## Extending

* Swap logging exporter with Jaeger, Prometheus, etc.
* Add metrics, logs, or node-level instrumentation.
