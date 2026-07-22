# Task 2 - Secure CI/CD Pipeline with GitOps

## Overview

This project implements a secure DevSecOps CI/CD pipeline using GitHub Actions. The pipeline performs automated security checks, builds a container image, generates a Software Bill of Materials (SBOM), signs the container image, publishes it to GitHub Container Registry (GHCR), and deploys the application to Kubernetes using Argo CD.

---

## Objective

The objective of this task was to implement a secure software delivery pipeline that enforces security throughout the software development lifecycle.

The pipeline automatically builds, scans, signs, and deploys the application while preventing insecure artifacts from reaching the Kubernetes cluster.

## Architecture

```
Developer Push
      │
      ▼
GitHub Actions
      │
      ├── Semgrep (SAST)
      ├── Gitleaks (Secret Scan)
      ├── Trivy Filesystem Scan
      ├── Docker Build
      ├── Trivy Image Scan
      ├── Push Image to GHCR
      ├── Generate CycloneDX SBOM (Syft)
      ├── Upload SBOM Artifact
      ├── Sign Container Image (Cosign)
      ├── Update GitOps Manifest
      └── Commit Changes
               │
               ▼
         GitHub Repository
               │
               ▼
            Argo CD
               │
               ▼
        Kubernetes Cluster
```

---

# Technologies Used

| Category | Tool |
|----------|------|
| CI/CD | GitHub Actions |
| Source Code Management | GitHub |
| Static Application Security Testing | Semgrep |
| Secret Detection | Gitleaks |
| Filesystem Vulnerability Scan | Trivy |
| Container Image Scan | Trivy |
| Containerization | Docker |
| Container Registry | GitHub Container Registry (GHCR) |
| SBOM Generation | Syft |
| Image Signing | Cosign |
| GitOps | Argo CD |
| Container Orchestration | Kubernetes |

---

# Repository Structure

```
task2-secure-cicd/
│
├── app/
│   ├── app.py
│   ├── Dockerfile
│   └── requirements.txt
│
├── argocd/
│   ├── application.yaml
│   ├── deployment.yaml
│   ├── service.yaml
│   └── kustomization.yaml
│
├── reports/
│
├── screenshots/
│
├── architecture.drawio
│
└── README.md
```

---

# CI/CD Pipeline Stages

## 1. Source Checkout

The pipeline checks out the latest source code from the GitHub repository.

---

## 2. Static Code Analysis

Semgrep performs static application security testing (SAST) to identify insecure coding practices.

---

## 3. Secret Scanning

Gitleaks scans the repository for hardcoded credentials, API keys, and secrets.

---

## 4. Filesystem Vulnerability Scan

Trivy scans the project filesystem for known vulnerabilities before building the container image.

---

## 5. Docker Build

The application is containerized using Docker.

---

## 6. Container Image Scan

Trivy scans the built Docker image for HIGH and CRITICAL vulnerabilities.

---

## 7. Publish Image

The container image is pushed to GitHub Container Registry (GHCR).

Image format:

```
ghcr.io/<github-username>/ledger-api:<commit-sha>
```

---

## 8. Software Bill of Materials (SBOM)

Syft generates a CycloneDX-compliant SBOM for the container image.

Generated file:

```
sbom-cyclonedx.json
```

The SBOM is uploaded as a GitHub Actions artifact.

---

## 9. Image Signing

Cosign signs the published container image using keyless signing.

---

## 10. GitOps Deployment

The workflow updates the Kubernetes deployment manifest with the latest image tag and commits the changes to the repository.

Argo CD automatically detects the change and synchronizes the Kubernetes cluster.

---

# Security Controls Implemented

- Static Application Security Testing (Semgrep)
- Secret Detection (Gitleaks)
- Filesystem Vulnerability Scanning
- Container Image Vulnerability Scanning
- SBOM Generation
- Container Image Signing
- GitOps-based Continuous Deployment

---

# Security Gate Policy

| Security Gate | Fail Policy |
|--------------|-------------|
| Semgrep | Fail on High severity findings |
| Gitleaks | Fail if any secret is detected |
| Trivy Filesystem Scan | Fail on Critical vulnerabilities |
| Trivy Image Scan | Fail on Critical vulnerabilities |
| Docker Build | Stop pipeline if build fails |
| GHCR Push | Stop deployment if image push fails |
| Cosign | Deployment proceeds only after successful image signing |
| Argo CD | Deployment occurs only after all previous stages complete |

For vulnerabilities without an available fix, the issue is documented, risk-assessed, and monitored until remediation becomes available.

# Deployment

Argo CD continuously monitors the Git repository.

Whenever a new image is published:

1. Deployment manifest is updated.
2. Git commit is created.
3. Argo CD detects the change.
4. Kubernetes deployment is synchronized automatically.

---

# Drift Detection & Self-Healing

Argo CD continuously compares the live Kubernetes cluster with the desired state stored in Git.

Verification steps:

1. Manually modify the Deployment using `kubectl edit`.
2. Observe the application status change to **OutOfSync**.
3. Argo CD automatically restores the desired configuration.
4. The application returns to the **Synced** and **Healthy** state.

# Verification

GitHub Actions

```bash
GitHub → Actions
```

Container Images

```bash
ghcr.io/<github-username>/ledger-api
```

Argo CD

```bash
kubectl get applications -n argocd
```

Kubernetes

```bash
kubectl get pods -n payments
```

SBOM

```bash
ls reports/
```

# Evidence

Screenshots are available in the `screenshots/` directory.

Include the following:

- Successful GitHub Actions pipeline
- GHCR package
- Trivy scan
- Semgrep scan
- Gitleaks scan
- CycloneDX SBOM artifact
- Argo CD Synced application
- Kubernetes deployment

---

# Results

The pipeline successfully implements a secure software supply chain with automated security validation, SBOM generation, container signing, GitOps deployment, and continuous delivery to Kubernetes.

---

# Assignment Coverage

| Requirement | Status |
|-------------|--------|
| GitHub Actions | ✅ |
| Docker Build | ✅ |
| Semgrep | ✅ |
| Gitleaks | ✅ |
| Trivy Filesystem Scan | ✅ |
| Trivy Image Scan | ✅ |
| GHCR Push | ✅ |
| CycloneDX SBOM | ✅ |
| Cosign Image Signing | ✅ |
| Argo CD GitOps | ✅ |
| Drift Detection | ✅ |
| Self-Healing | ✅ |
