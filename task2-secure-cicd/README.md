# Task 2 - Secure CI/CD Pipeline with GitOps

## Overview

This project implements a secure DevSecOps CI/CD pipeline using GitHub Actions. The pipeline performs automated security checks, builds a container image, generates a Software Bill of Materials (SBOM), signs the container image, publishes it to GitHub Container Registry (GHCR), and deploys the application to Kubernetes using Argo CD.

---

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

# Deployment

Argo CD continuously monitors the Git repository.

Whenever a new image is published:

1. Deployment manifest is updated.
2. Git commit is created.
3. Argo CD detects the change.
4. Kubernetes deployment is synchronized automatically.

---

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

# Future Improvements

- SLSA Provenance Generation
- Admission Controller Verification
- Policy-as-Code using Kyverno
- Automated Dependency Updates
- Continuous Compliance Reporting
