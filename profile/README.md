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

### AIOps — AI-Powered Code Review & Log Analysis

Two AI components running inside the cluster, powered by **Claude API (Haiku)**:

```
┌──────────────────────────────────────────────────────────────────────┐
│                         AIOps Module                                  │
│                                                                       │
│  ┌─ Code Review Gateway (always running) ────────────────────────┐   │
│  │                                                                │   │
│  │  Jenkins CI ──► POST /review ──► FastAPI ──► Claude API       │   │
│  │  (code diff)        │                          │               │   │
│  │                     │              security issues,            │   │
│  │                     │              quality feedback,           │   │
│  │                     ◄──────────── recommendations              │   │
│  │                                                                │   │
│  │  Prometheus ◄── /metrics (pending requests gauge)             │   │
│  └────────────────────────────────────────────────────────────────┘   │
│                                                                       │
│  ┌─ Log Analyzer CronJob (daily 3:00 UTC) ───────────────────────┐   │
│  │                                                                │   │
│  │  Loki ──► ERROR logs (24h) ──► deduplicate ──► Claude API     │   │
│  │             (500 lines)      (10 unique patterns)   │          │   │
│  │                                                     ▼          │   │
│  │                                              Slack report      │   │
│  │                                          (root cause + fixes)  │   │
│  └────────────────────────────────────────────────────────────────┘   │
│                                                                       │
│  Secrets: AWS Secrets Manager → ExternalSecret → K8s Secret          │
│  Dashboard: Grafana (gateway status, pending requests, history)      │
└──────────────────────────────────────────────────────────────────────┘
```

| Component | Technology | How it works |
|---|---|---|
| **Code Review Gateway** | FastAPI + Claude API (Haiku) | Jenkins sends code diffs on every CI build → AI returns security/quality review |
| **Nightly Analysis** | Same gateway | Jenkins sends build metrics (tests, coverage, mutations, CVEs, k6 perf) → AI provides trend analysis |
| **Log Analyzer** | Python CronJob + Loki + Claude API | Daily: queries ERROR logs → deduplicates → AI root cause analysis → Slack report |
| **API Key** | External Secrets Operator | Zero hardcoded credentials — synced from AWS Secrets Manager |
| **Monitoring** | Prometheus + Grafana | Dedicated AIOps dashboard with gateway status and request metrics |

**Why Claude API, not a local model?** Originally ran Ollama (qwen2.5-coder:7b) on a GPU SPOT node (g4dn.xlarge). Replaced for three reasons:
1. **Cost** — GPU node ~$120/month vs Claude API Haiku ~$2/month (60x cheaper)
2. **Quality** — 7B model gives generic reviews; Haiku catches real logic bugs and security issues
3. **Ops overhead** — no GPU drivers, CUDA, model updates, or OOM debugging

**Security:** only source code diffs are sent (no customer data, no secrets). Anthropic API has a [zero data retention policy](https://www.anthropic.com/policies/privacy). All AI stages are non-blocking — gateway downtime never breaks CI/CD.

### Platform

- **ArgoCD App of Apps** — 18 managed applications, canary deployments (auto-rollback on >5% errors)
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
