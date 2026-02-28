# Synaptix Innovations

Production-grade DevOps platforms and cloud infrastructure.

---

## Borod Bank — GitOps/AIOps Platform

End-to-end **GitOps/AIOps platform** on AWS EKS: three-tier CI/CD pipeline, AI-powered code review, canary deployments with auto-rollback, and full observability stack. Solo DevOps project — one person, zero manual operations.

[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.32-326CE5?logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Terraform](https://img.shields.io/badge/Terraform-1.5+-7B42BC?logo=terraform&logoColor=white)](https://www.terraform.io/)
[![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?logo=jenkins&logoColor=white)](https://www.jenkins.io/)
[![ArgoCD](https://img.shields.io/badge/ArgoCD-GitOps-EF7B4D?logo=argo&logoColor=white)](https://argo-cd.readthedocs.io/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.5-6DB33F?logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![AWS EKS](https://img.shields.io/badge/AWS-EKS-FF9900?logo=amazonaws&logoColor=white)](https://aws.amazon.com/eks/)

### Three-Tier CI/CD

| Tier | Trigger | What it does |
|---|---|---|
| **GitHub Actions** | Every PR | Gitleaks, Hadolint, Trivy, compile + test (gate checks) |
| **Jenkins CI (20+ stages)** | Merge to main | Full build, 6 security scanners, SBOM, Cosign, deploy |
| **Jenkins Nightly (14 stages)** | Daily 2 AM | Mutation testing, k6 perf, ZAP DAST, Nuclei, AI code review |

### Platform

- **ArgoCD App of Apps** — 18 managed applications, canary deployments (auto-rollback on >5% errors)
- **AIOps** — Claude API + FastAPI gateway for automated nightly code review (migrated from local Ollama 7B — 60x cheaper, better quality)
- **Monitoring** — Prometheus + Grafana (5 dashboards) + 17 nightly metrics via Pushgateway
- **Infrastructure** — 1,300+ lines of Terraform: EKS, RDS, KMS, EFS, 6 IRSA roles, VPC flow logs
- **Security** — Kyverno policies, Cosign image signing, SBOM, Chaos Mesh, External Secrets Operator

### Repositories

| Repository | Description |
|---|---|
| **[borod-bank-docs](https://github.com/Synaptix-Innovations/borod-bank-docs)** | Architecture overview, screenshots, ADRs |
| **[borod-bank-ci](https://github.com/Synaptix-Innovations/borod-bank-ci)** | Spring Boot app + Jenkinsfile (20+ stages) + Jenkinsfile.nightly (14 stages) |
| **[borod-bank-cd](https://github.com/Synaptix-Innovations/borod-bank-cd)** | Kubernetes manifests + ArgoCD App of Apps (18 apps) |
| **[borod-bank-infra](https://github.com/Synaptix-Innovations/borod-bank-infra)** | Terraform IaC: VPC, EKS, RDS, KMS, EFS, IRSA |
| **[jenkins-setup](https://github.com/Synaptix-Innovations/jenkins-setup)** | Custom Jenkins controller: Dockerfile + JCasC + GitHub Actions |
