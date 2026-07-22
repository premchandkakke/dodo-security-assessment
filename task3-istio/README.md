# Task 3 - Service Mesh Security using Istio

## Overview

This task demonstrates the implementation of a secure service mesh using **Istio** on a Kubernetes cluster. The objective is to provide secure service-to-service communication, traffic management, and policy enforcement for the **ledger-api** application deployed in the **payments** namespace.

The service mesh provides:

- Automatic sidecar proxy injection
- Secure communication using Mutual TLS (mTLS)
- Ingress traffic management
- Authorization policies
- Service discovery
- Traffic routing through Istio Gateway

---

# Architecture

```
                    +-----------------------+
                    |       Developer       |
                    +-----------+-----------+
                                |
                                |
                                v
                     +----------------------+
                     |      GitHub Repo     |
                     +-----------+----------+
                                 |
                                 |
                                 v
                          GitHub Actions
                                 |
                                 |
                                 v
                             Argo CD
                                 |
                                 |
                                 v
                    Kubernetes Cluster (Kind)
                                 |
        -------------------------------------------------
        |                                               |
        |                                               |
        v                                               v
 Istio Ingress Gateway                          Istiod Control Plane
        |                                               |
        |                                               |
        +-------------------+---------------------------+
                            |
                            |
                            v
                 ledger-api Deployment
                +-----------------------+
                |   ledger-api          |
                |   istio-proxy         |
                +-----------------------+
                            |
                            |
                            v
                    Kubernetes Service
```

---

# Technologies Used

| Component | Technology |
|------------|------------|
| Container Orchestration | Kubernetes |
| Service Mesh | Istio 1.30 |
| GitOps | Argo CD |
| CI/CD | GitHub Actions |
| Container Registry | GitHub Container Registry (GHCR) |
| Security | Mutual TLS (mTLS) |
| Policy Enforcement | AuthorizationPolicy |
| Routing | Gateway & VirtualService |

---

# Project Structure

```
task3-istio/
│
├── manifests/
│   ├── gateway.yaml
│   ├── virtualservice.yaml
│   ├── destinationrule.yaml
│   ├── peerauthentication.yaml
│   └── authorizationpolicy.yaml
│
├── networkpolicy/
│
├── policies/
│
├── screenshots/
│
├── architecture.drawio
│
└── README.md
```

---

# Components Implemented

## 1. Istio Installation

Istio was installed using the Demo profile.

Installed components:

- Istiod
- Istio Ingress Gateway
- Istio Egress Gateway

Automatic sidecar injection was enabled for the `payments` namespace.

Features enabled:

- Istiod
- Ingress Gateway
- Egress Gateway
- CNI Plugin

---

## 2. Automatic Sidecar Injection

The **payments** namespace was configured for automatic sidecar injection.

```
kubectl label namespace payments istio-injection=enabled
```

Every application pod automatically receives an Envoy proxy.

---

## 3. Istio Gateway

An Istio Gateway exposes the application through the Istio Ingress Gateway.

Resource:

```
gateway.yaml
```

Purpose:

- External access
- HTTP traffic entry point

---

## 4. Virtual Service

A VirtualService defines traffic routing rules.

Resource:

```
virtualservice.yaml
```

Responsibilities:

- Request routing
- URL matching
- Service forwarding

---

## 5. Destination Rule

DestinationRule defines communication policies between services.

Resource:

```
destinationrule.yaml
```

Configured:

- ISTIO_MUTUAL TLS mode

---

## 6. Mutual TLS

PeerAuthentication enables secure encrypted communication.

Resource:

```
peerauthentication.yaml
```

Configuration:

```
Mode: STRICT
```

Benefits:

- Encrypted service communication
- Identity verification
- Zero Trust networking

---

## 7. Authorization Policy

AuthorizationPolicy restricts traffic reaching the application.

Resource:

```
authorizationpolicy.yaml
```

Policy:

- Allow requests originating from the **payments** namespace.

---

# Validation

The following validations were performed.

## Verify Istio Installation

```
kubectl get pods -n istio-system
```

---

## Verify Sidecar Injection

```
kubectl describe pod <pod-name>
```

Verified:

- ledger-api
- istio-proxy

---

## Verify Envoy Proxy

```
istioctl proxy-status
```

Verified:

- Proxy connected to Istiod
- CDS
- LDS
- EDS
- RDS

---

## Verify Gateway

```
kubectl get gateway -n payments
```

---

## Verify Virtual Service

```
kubectl get virtualservice -n payments
```

---

## Verify Destination Rule

```
kubectl get destinationrule -n payments
```

---

## Verify Peer Authentication

```
kubectl get peerauthentication -n payments
```

---

## Verify Authorization Policy

```
kubectl get authorizationpolicy -n payments
```

---

# Security Features

✔ Automatic Sidecar Injection

✔ Mutual TLS (STRICT)

✔ Authorization Policy

✔ Service-to-Service Encryption

✔ Envoy Proxy

✔ Zero Trust Networking

✔ Secure Ingress Gateway

---

# Defense in Depth

The deployment combines Kubernetes NetworkPolicies with Istio security policies.

| Kubernetes NetworkPolicy | Istio Service Mesh |
|--------------------------|--------------------|
| Controls Pod-to-Pod network connectivity | Controls application-layer (L7) traffic |
| Enforces network segmentation | Enforces workload identity |
| Operates at Layer 3/4 | Operates at Layer 7 |
| Blocks unauthorized network connections | Authorizes requests based on authenticated workload identity |

Using both mechanisms provides layered security by protecting both the network and application layers.

# Deployment Flow

```
Developer

      │

      ▼

GitHub Repository

      │

      ▼

GitHub Actions

      │

      ▼

Argo CD

      │

      ▼

Kubernetes

      │

      ▼

Istio Gateway

      │

      ▼

Virtual Service

      │

      ▼

ledger-api + Envoy Sidecar

      │

      ▼

Secure Service Communication
```

---

# Screenshots

Include the following screenshots in the **screenshots/** directory.

```
screenshots/

├── istio-install.png
├── sidecar-injection.png
├── proxy-status.png
├── gateway.png
├── virtualservice.png
├── destinationrule.png
├── peerauthentication.png
├── authorizationpolicy.png
├── ledger-api-pod.png
└── ingress-test.png
```

---

# Assignment Requirements Coverage

| Requirement | Status |
|-------------|--------|
| Install Istio | ✅ |
| Automatic Sidecar Injection | ✅ |
| Gateway | ✅ |
| VirtualService | ✅ |
| DestinationRule | ✅ |
| PeerAuthentication (STRICT mTLS) | ✅ |
| AuthorizationPolicy | ✅ |
| Kubernetes NetworkPolicy | ✅ |

# Results

Successfully implemented an Istio-based service mesh providing:

- Secure service-to-service communication
- Automatic sidecar injection
- Mutual TLS encryption
- Layer-7 traffic routing
- Authorization policy enforcement
- Service discovery
- Secure ingress traffic management

---



# Workload Identity

Istio automatically issues X.509 certificates to workloads through `istiod`.

Each workload receives a unique SPIFFE identity, which is used for mutual authentication within the service mesh.

The trust root is managed by Istio's internal Certificate Authority (CA). Workload certificates are rotated automatically before expiration, ensuring secure and continuous identity management without manual intervention.

# Conclusion

This implementation demonstrates a production-oriented Istio service mesh integrated with Kubernetes and GitOps workflows. It enables secure communication, centralized traffic management, policy enforcement, and observability while following modern cloud-native security best practices.
