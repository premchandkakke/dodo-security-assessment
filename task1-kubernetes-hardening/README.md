# Task 1 – Kubernetes Hardening

## Objective

Deploy the provided ledger-api application on Kubernetes and transform it into a production-grade deployment following Kubernetes security best practices.

---

# Architecture

The application consists of:

- ledger-api
- reporting (neighbour service)
- ConfigMap
- Sealed Secret
- Dedicated ServiceAccounts
- RBAC
- NGINX Ingress
- Kyverno Admission Policies

Architecture Diagram:

```
architecture.drawio
```

---

# Kubernetes Components

## Namespace

Namespace:

```
payments
```

Pod Security Standard:

```
restricted
```

---

## Deployments

### ledger-api

Security Hardening:

- Runs as non-root
- Read-only root filesystem
- RuntimeDefault seccomp
- Linux capabilities dropped
- Resource requests
- Resource limits
- Liveness Probe
- Readiness Probe

### reporting

Neighbour service deployed with identical security controls.

---

# RBAC

Dedicated Service Accounts

- ledger-api-sa
- reporting-sa

Least Privilege RBAC implemented.

Default ServiceAccount not used.

---

# ConfigMap

Application configuration stored in ConfigMap.

---

# Secrets

Sensitive configuration stored using Bitnami Sealed Secrets.

Plain Kubernetes Secret removed from repository.

---

# Ingress

NGINX Ingress Controller deployed.

Ingress Host:

```
ledger.local
```

Verified using:

```bash
kubectl port-forward -n ingress-nginx service/ingress-nginx-controller 8088:80

curl -H "Host: ledger.local" http://localhost:8088/health
```

---

# Security Hardening

Implemented:

- runAsNonRoot
- allowPrivilegeEscalation=false
- readOnlyRootFilesystem=true
- seccomp RuntimeDefault
- Drop ALL Linux Capabilities
- Dedicated ServiceAccount
- Least Privilege RBAC

---

# Resource Limits

CPU Requests

```
100m
```

CPU Limits

```
500m
```

Memory Requests

```
128Mi
```

Memory Limits

```
256Mi
```

---

# Health Checks

Readiness Probe

```
/health
```

Liveness Probe

```
/health
```

---

# Kyverno

Installed Kyverno Admission Controller.

Policies implemented:

- Reject latest image tags
- Reject root containers

Verified policy enforcement successfully.

---

# Pod Security Standards

Namespace configured with:

```
restricted
```

Verified insecure workloads are rejected.

---

# Testing

Deployment

```bash
kubectl get deployment -n payments
```

Pods

```bash
kubectl get pods -n payments
```

Services

```bash
kubectl get svc -n payments
```

Health Check

```bash
curl http://localhost:8080/health
```

Ingress

```bash
curl -H "Host: ledger.local" http://localhost:8088/health
```

---

# Screenshots

Screenshots available under

```
screenshots/
```

Examples:

- application-health-check.png
- deployment-service-running.png
- kyverno-policy-rejection.png
- pod-security-rejection.png

---

# Security Improvements

Compared to the original deployment:

- Removed hardcoded secrets
- Introduced Sealed Secrets
- Hardened containers
- Restricted Pod Security
- Implemented Least Privilege RBAC
- Enforced Admission Policies
- Added Health Probes
- Added Resource Requests & Limits
- Introduced NGINX Ingress

---

# Result

The application is successfully deployed on a local Kind Kubernetes cluster with production-oriented security controls and policy enforcement.
