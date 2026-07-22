# Dodo Payments Security Assessment

This repository contains my implementation of the Dodo Payments Security & DevOps Engineer Technical Assessment.

The objective of this assessment was to demonstrate practical skills in Kubernetes hardening, secure CI/CD, GitOps, service mesh security, zero-trust networking, software supply chain security, and authorized penetration testing using a fully local environment.

## Repository Structure

```
dodo-security-assessment/
├── README.md
├── task1-kubernetes-hardening/
├── task2-secure-cicd/
├── task3-istio/
└── task4-pentest/
```

---

# Task 1 – Kubernetes Hardening

Implemented:

- Hardened Kubernetes Deployment
- Dedicated ServiceAccount
- Least Privilege RBAC
- ConfigMap
- Sealed Secrets
- Ingress
- Kyverno Policies
- Pod Security Standards (Restricted)
- Resource Requests & Limits
- Liveness & Readiness Probes

Documentation:

```
task1-kubernetes-hardening/README.md
```

---

# Task 2 – Secure CI/CD

Implemented:

- GitHub Actions CI/CD Pipeline
- Docker Image Build & Push to GitHub Container Registry (GHCR)
- Semgrep Static Application Security Testing (SAST)
- Trivy Dependency and Container Image Scanning
- Gitleaks Secret Scanning
- CycloneDX SBOM Generation
- Cosign Keyless Image Signing
- Argo CD GitOps Deployment
- Drift Detection & Self-Healing

---

# Task 3 – Istio Zero Trust

Implemented:

- Istio Service Mesh Installation
- Automatic Sidecar Injection
- STRICT Mutual TLS (mTLS)
- Istio Gateway
- VirtualService
- DestinationRule
- AuthorizationPolicy
- Kubernetes NetworkPolicy (Defense in Depth)


---

# Task 4 – Reconnaissance & Penetration Testing

Implemented:

### Passive Reconnaissance

- Certificate Transparency Log Enumeration
- DNS Enumeration
- Subdomain Discovery (Subfinder, Assetfinder, Amass)
- Live Host Discovery (HTTPX)
- Technology Fingerprinting (WhatWeb)
- TLS Configuration Assessment (testssl.sh)

### Authorized Penetration Testing

Performed against the locally deployed `ledger-api` application.

Security assessment included:

- Endpoint Enumeration
- Sensitive Data Exposure Assessment
- SSRF Assessment
- Input Validation Review
- YAML Parsing Review
- Security Header Inspection
- Proof-of-Concept Validation
- CVSS-Based Risk Assessment
- Remediation Recommendations

Documentation:

```
task4-pentest/README.md
```

## How to Navigate

Each task contains:

- README.md describing the implementation
- Kubernetes manifests or workflow files
- Architecture diagrams
- Screenshots demonstrating successful execution
- Supporting reports and evidence


## Technologies Used

- Kubernetes (Kind)
- Docker
- GitHub Actions
- GitHub Container Registry (GHCR)
- Argo CD
- Istio
- NGINX Ingress Controller
- Kyverno
- Sealed Secrets
- Trivy
- Semgrep
- Gitleaks
- Cosign
- CycloneDX
- HTTPX
- WhatWeb
- Subfinder
- Assetfinder
- Amass
- testssl.sh
- Nuclei
- ffuf
- Burp Suite Community




## High-Level Architecture

```
Developer
      │
      ▼
GitHub Repository
      │
      ▼
GitHub Actions
      │
      ├── Semgrep
      ├── Trivy
      ├── Gitleaks
      ├── SBOM
      ├── Cosign
      ▼
GitHub Container Registry
      │
      ▼
Argo CD
      │
      ▼
Kubernetes (Kind)
      │
      ├── Kyverno
      ├── NGINX Ingress
      ├── Istio
      └── ledger-api
```
