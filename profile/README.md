# Synaptix Innovations

We build production-grade DevOps platforms and cloud infrastructure.

---

## Flagship Project: Borod Bank

A fully automated **GitOps/AIOps platform** on AWS EKS — from Terraform provisioning through CI/CD pipelines to AI-powered code review and production monitoring. Built as a solo DevOps project demonstrating end-to-end platform engineering.

[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.32-326CE5?logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Terraform](https://img.shields.io/badge/Terraform-1.5+-7B42BC?logo=terraform&logoColor=white)](https://www.terraform.io/)
[![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?logo=jenkins&logoColor=white)](https://www.jenkins.io/)
[![ArgoCD](https://img.shields.io/badge/ArgoCD-GitOps-EF7B4D?logo=argo&logoColor=white)](https://argo-cd.readthedocs.io/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.5-6DB33F?logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![AWS EKS](https://img.shields.io/badge/AWS-EKS-FF9900?logo=amazonaws&logoColor=white)](https://aws.amazon.com/eks/)

### Highlights

- **20+ stage Jenkins CI pipeline** with 6 security scanning tools
- **14-stage nightly regression** — mutation testing, architecture tests, k6 perf, ZAP DAST, Nuclei pen testing, AI code review
- **ArgoCD App of Apps** managing 18 applications with canary deployments (auto-rollback on >5% errors)
- **1,300+ lines of Terraform** — EKS, RDS, KMS, EFS, IRSA, VPC flow logs
- **5 Grafana dashboards** + Prometheus monitoring + Slack notifications
- **AIOps** — Claude API + FastAPI gateway for automated code review

### Repositories

| Repository | Description |
|---|---|
| **[borod-bank-docs](https://github.com/Synaptix-Innovations/borod-bank-docs)** | Project overview, screenshots, architecture decision records |
| **[borod-bank-ci](https://github.com/Synaptix-Innovations/borod-bank-ci)** | Spring Boot app + Jenkins CI/CD pipelines |
| **[borod-bank-cd](https://github.com/Synaptix-Innovations/borod-bank-cd)** | Kubernetes manifests + ArgoCD configuration |
| **[borod-bank-infra](https://github.com/Synaptix-Innovations/borod-bank-infra)** | Terraform IaC for AWS infrastructure |
| **[jenkins-setup](https://github.com/Synaptix-Innovations/jenkins-setup)** | Custom Jenkins controller image |
