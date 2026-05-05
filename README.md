<h1 align="center">🚀 Production-Grade DevSecOps GitOps Pipeline</h1>

<p align="center">
  <b>Code → Scan → Build → Deploy → Promote → Observe → Scale</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/CI-GitHub%20Actions-181717?style=for-the-badge&logo=githubactions&logoColor=white" />
  <img src="https://img.shields.io/badge/CD-ArgoCD-EF7B4D?style=for-the-badge&logo=argo&logoColor=white" />
  <img src="https://img.shields.io/badge/Kubernetes-Orchestration-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white" />
  <img src="https://img.shields.io/badge/Security-Trivy%20%7C%20Gitleaks%20%7C%20Semgrep-1904DA?style=for-the-badge&logo=aqua&logoColor=white" />
  <img src="https://img.shields.io/badge/Observability-Prometheus%20%2B%20Grafana-E6522C?style=for-the-badge&logo=prometheus&logoColor=white" />
  <img src="https://img.shields.io/badge/IaC-Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white" />
  <img src="https://img.shields.io/badge/Delivery-Canary%20%7C%20Blue--Green-0F766E?style=for-the-badge" />
</p>

---

## 💡 Project Summary

A production-grade DevSecOps pipeline implementing GitOps, three-environment progressive delivery with PR-based promotion gates, shift-left security (Trivy + Gitleaks + Semgrep), canary and blue-green deployment strategies, real-time observability, and infrastructure provisioned as code using Terraform — all running on Kubernetes.

---

## ✨ Overview

This project demonstrates a **complete, production-style DevSecOps workflow** from developer commit to live cluster — with enterprise-level security, deployment safety, and observability built in at every stage.

**What makes this different from a basic CI/CD project:**

- Security is enforced at **three separate layers** before any image reaches a cluster
- Deployments follow a **controlled promotion model** — no direct pushes to production
- Progressive delivery strategies eliminate **zero-downtime** releases
- Every piece of infrastructure is **version-controlled and reproducible** via Terraform

---

## 🏗️ Architecture

<!-- 📸 ADD YOUR ARCHITECTURE DIAGRAM HERE -->
<!-- Recommended: export from draw.io or Excalidraw as PNG -->
<!-- Size: 900px wide minimum for clarity -->
<!-- File: docs/screenshots/architecture.png -->

```
[ INSERT ARCHITECTURE DIAGRAM IMAGE HERE ]
```

> Suggested tool: draw.io, Excalidraw, or Lucidchart  
> Show: GitHub → GitHub Actions → Docker Hub → ArgoCD → K8s (dev/staging/prod namespaces) → Prometheus → Grafana

---

## 🔄 CI Pipeline Flow

> Every commit triggers a full security gate before any image is built or pushed.

<!-- 📸 ADD YOUR GITHUB ACTIONS PIPELINE SCREENSHOT HERE -->
<!-- Show: all steps passing (green checks) in the Actions tab -->
<!-- File: docs/screenshots/github-actions-pipeline.png -->

```
[ INSERT GITHUB ACTIONS PIPELINE SCREENSHOT — all green checks ]
```

**Stage-by-stage breakdown:**

| Stage | Tool | What it does | Fails pipeline if |
|---|---|---|---|
| Secrets scan | Gitleaks | Detects hardcoded API keys, tokens, passwords | Any secret found in code |
| Code analysis | Semgrep | Detects SQL injection, XSS, logic flaws | CRITICAL/HIGH rule match |
| Image build | Docker | Builds container from Dockerfile | Build error |
| Image scan | Trivy | Scans image for known CVEs | CRITICAL CVE found |
| Push | Docker Hub | Pushes verified, clean image | Only reached if all above pass |
| Helm update | Git | Bumps image tag in values file | Triggers ArgoCD sync |

---

## 🌍 Multi-Environment Deployment

Three isolated environments, each with its own Helm values, resource limits, and deployment strategy.

<!-- 📸 ADD ARGOCD SCREENSHOT HERE -->
<!-- Show: all 3 apps (dev, staging, prod) in Synced/Healthy state -->
<!-- File: docs/screenshots/argocd-three-envs.png -->

```
[ INSERT ARGOCD DASHBOARD SCREENSHOT — 3 environments synced ]
```

| Environment | Trigger | Replicas | Strategy | Values file |
|---|---|---|---|---|
| **Dev** | Auto on every merge | 1 | Rolling update | `values-dev.yaml` |
| **Staging** | PR approval required | 2 | Canary | `values-staging.yaml` |
| **Production** | Manual approval required | 3 | Blue-Green | `values-prod.yaml` |

---

## 🔁 Promotion Pipeline

A GitOps-based promotion workflow ensures no code reaches production without passing through validation gates.

```
Developer pushes code
        │
        ▼
  CI runs (Gitleaks → Semgrep → Build → Trivy)
        │
        ▼ (all gates pass)
  Image pushed to Docker Hub
        │
        ▼
  Helm values updated → ArgoCD detects change
        │
        ▼
  ┌─────────────┐
  │     DEV     │ ← Auto deploy (no approval needed)
  └─────────────┘
        │
        ▼ (manual PR raised for staging)
  ┌─────────────┐
  │   STAGING   │ ← PR review + approval required
  └─────────────┘
        │
        ▼ (manual approval in GitHub Actions)
  ┌─────────────┐
  │  PRODUCTION │ ← Manual approval gate
  └─────────────┘
```

This ensures every production release is intentional, reviewed, and traceable in Git history.

---

## 🔐 Shift-Left Security

Security is enforced at multiple stages — not just at deployment.

<!-- 📸 ADD TRIVY SCAN OUTPUT SCREENSHOT HERE -->
<!-- Show: Trivy blocking a vulnerable image (exit code 1) OR clean scan passing -->
<!-- File: docs/screenshots/trivy-scan-result.png -->

```
[ INSERT TRIVY SCAN OUTPUT — showing blocked or clean result ]
```

| Tool | Stage | Detects | Blocks pipeline |
|---|---|---|---|
| **Gitleaks** | Pre-build | Hardcoded secrets, API keys, tokens | ✅ Yes |
| **Semgrep** | Pre-build | SQL injection, XSS, insecure patterns | ✅ Yes |
| **Trivy** | Post-build | Container CVEs, OS vulnerabilities | ✅ Yes (CRITICAL) |

**Why this matters:** A vulnerability caught at commit time costs minutes to fix. The same vulnerability caught in production costs hours of downtime and potential data breach.

---

## 🚀 Progressive Delivery

Advanced deployment strategies are implemented to reduce risk during every release.

<!-- 📸 ADD ARGO ROLLOUTS / DEPLOYMENT SCREENSHOT HERE -->
<!-- Show: canary traffic split or blue-green switch in progress -->
<!-- File: docs/screenshots/progressive-delivery.png -->

```
[ INSERT CANARY OR BLUE-GREEN DEPLOYMENT SCREENSHOT ]
```

**Canary deployment (Staging):**
- New version receives a small % of traffic first
- Metrics are observed before full rollout
- Automatic rollback if error rate spikes

**Blue-Green deployment (Production):**
- Two identical environments run in parallel
- Traffic switches instantly to the new version
- Old version kept live for instant rollback if needed
- Zero-downtime guaranteed

---

## 🧰 Tech Stack

| Category | Tools | Purpose |
|---|---|---|
| CI | GitHub Actions | Automated build, scan, and push pipeline |
| CD / GitOps | ArgoCD | Git-driven cluster reconciliation |
| Containerization | Docker | Image build and packaging |
| Orchestration | Kubernetes | Container scheduling and management |
| Infrastructure | Terraform | Cluster and cloud infra as code |
| Packaging | Helm | Per-environment config management |
| Security | Trivy, Gitleaks, Semgrep | Shift-left security at every stage |
| Monitoring | Prometheus, Grafana | Metrics collection and dashboards |
| Alerting | Alertmanager, Slack | Incident notification |
| Scaling | Kubernetes HPA | CPU-based auto-scaling |
| Delivery | Argo Rollouts / K8s | Canary and blue-green strategies |

---

## 📊 Observability

<!-- 📸 ADD GRAFANA DASHBOARD SCREENSHOT HERE -->
<!-- Show: a real dashboard with metrics (request rate, latency, pod count, CPU) -->
<!-- File: docs/screenshots/grafana-dashboard.png -->

```
[ INSERT GRAFANA DASHBOARD SCREENSHOT — real metrics visible ]
```

<!-- 📸 ADD SLACK ALERT SCREENSHOT HERE -->
<!-- Show: Alertmanager firing an alert to your Slack channel -->
<!-- File: docs/screenshots/slack-alert.png -->

```
[ INSERT SLACK ALERT SCREENSHOT ]
```

- **Prometheus** scrapes application and cluster metrics on a defined scrape interval
- **Grafana** provides real-time dashboards (request rate, error rate, latency, pod health)
- **Alertmanager** routes critical alerts to Slack with rule-based thresholds
- **ServiceMonitor** CRDs automatically register new services for scraping

---

## ⚙️ Auto Scaling

- Horizontal Pod Autoscaler is configured per environment
- Pods scale based on CPU utilization thresholds
- Dev: min 1 / max 3 pods | Staging: min 2 / max 5 | Production: min 3 / max 10

---

## 🏗️ Infrastructure as Code

The Kubernetes cluster and supporting cloud infrastructure are provisioned using **Terraform** — not manual console clicks.

```bash
# Provision the full infrastructure
cd terraform/
terraform init
terraform plan
terraform apply
```

This ensures the entire environment is reproducible, version-controlled, and auditable.

---

## 📁 Project Structure

```text
Production-Grade-DevSecOps-GitOps-Pipeline/
|-- .github/
|   `-- workflows/
|       `-- ci.yml
|-- app/
|   |-- app.js
|   |-- Dockerfile
|   `-- package.json
|-- argocd/
|   |-- app-dev.yaml
|   |-- app-prod.yaml
|   `-- app-staging.yaml
|-- gitops-chart/
|   |-- Chart.yaml
|   |-- values.yaml
|   |-- values-dev.yaml
|   |-- values-prod.yaml
|   |-- values-staging.yaml
|   `-- templates/
|       |-- _helpers.tpl
|       |-- deployment.yaml
|       |-- rollout.yaml
|       |-- rollout-bluegreen.yaml
|       |-- service.yaml
|       |-- hpa.yaml
|       |-- ingress.yaml
|       |-- servicemonitor.yaml
|       |-- alert-rule.yaml
|       |-- serviceaccount.yaml
|       `-- tests/
|-- monitoring/
|   `-- alertmanager-config.yaml
`-- README.md
```

---

## 🧪 Quick Start

```bash
# 1. Provision infrastructure
cd terraform && terraform apply

# 2. Start local cluster (if not using cloud)
minikube start

# 3. Deploy ArgoCD apps for all environments
kubectl apply -f argocd/application-dev.yaml
kubectl apply -f argocd/application-staging.yaml
kubectl apply -f argocd/application-prod.yaml

# 4. Verify all workloads are running
kubectl get pods -A

# 5. Access Grafana dashboard
kubectl port-forward svc/grafana 3000:3000 -n monitoring
```

---

## 🧠 Architecture Highlights

- **Git is the single source of truth** — all changes go through Git, ArgoCD reconciles the cluster to match
- **Security enforced at three layers** — code (Semgrep), secrets (Gitleaks), image (Trivy)
- **No direct production pushes** — every prod deploy requires a manual approval gate
- **Infrastructure is reproducible** — Terraform provisions everything from scratch
- **Deployment strategies reduce blast radius** — canary and blue-green limit production risk

---

## 🔮 Future Enhancements

- OPA/Kyverno policy-as-code for Kubernetes admission control
- Multi-cluster management with ArgoCD ApplicationSets

---

## 👨‍💻 Author

**Gotam Kumar Prajapati**  
[GitHub](https://github.com/Wiesslogia) · [LinkedIn](https://www.linkedin.com/in/gotamprajapati/) 