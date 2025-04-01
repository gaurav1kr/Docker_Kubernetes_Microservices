
# 🔐 Secure CI/CD Pipeline with GitHub Actions + BlackDuck + SonarQube

This guide outlines how to design a secure CI/CD pipeline using GitHub Actions, integrated with BlackDuck for OSS compliance and SonarQube for static code analysis.

---

## 🧱 1. Core Pipeline Stages

| Stage                 | Description                                           |
|----------------------|-------------------------------------------------------|
| ✅ Code Commit        | Developer pushes code to GitHub                       |
| 🧪 Static Analysis    | Run SonarQube scan for bugs, code smells, etc.        |
| 🛡️ License & CVE Scan | Run BlackDuck scan for OSS risks                      |
| 🐳 Build & Package     | Build Docker image or binary                          |
| 🔐 Image Scan & Sign  | Scan/sign image (Trivy, Cosign)                       |
| 🚀 Deploy             | GitOps or direct deploy to OpenShift/Kubernetes       |
| 📓 Audit & Notify     | Post results to GitHub, Slack, Jira                   |

---

## 🧪 2. GitHub Actions Pipeline - Example

```yaml
name: Secure CI/CD Pipeline

on: [push, pull_request]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: SonarQube Scan
        uses: sonarsource/sonarqube-scan-action@v1
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: https://sonarqube.myorg.com

      - name: BlackDuck Scan
        run: |
          ./blackduck-scan.sh --url $BLACKDUCK_URL --token $BLACKDUCK_TOKEN

      - name: Build Docker Image
        run: |
          docker build -t myapp:${{ github.sha }} .

      - name: Image Signing (optional)
        run: cosign sign --key cosign.key myapp:${{ github.sha }}

      - name: Deploy to Staging
        run: ./deploy-to-openshift.sh

      - name: Notify
        uses: slackapi/slack-github-action@v1.24.0
```

---

## 🔒 3. Security Best Practices

### GitHub Actions Security:
- Store all tokens in **GitHub Secrets**
- Use OIDC for federated cloud/Vault access
- Restrict direct push to `main` using branch protections

```yaml
env:
  BLACKDUCK_TOKEN: ${{ secrets.BLACKDUCK_TOKEN }}
  SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

---

## 🔍 4. BlackDuck Integration

- Scan for license violations and CVEs
- Enforce policies to block deploy on violations

---

## 📊 5. SonarQube Integration

- Code smells, bugs, test coverage, and security hotspots
- Add quality gates to block merge if scan fails

---

## 🧰 6. Toolchain Architecture Diagram

```
[GitHub PR] --> [GitHub Actions Workflow]
                      |
              +-------+--------+
              | SonarQube Scan |
              | BlackDuck Scan |
              +-------+--------+
                      |
             [Docker Build + Optional Sign]
                      |
                [OpenShift Deploy]
                      |
               [Slack/Jira Notification]
```

---

## ✅ Optional Enhancements

- 🔄 Pre-commit hooks
- 🔐 Cosign + Kyverno enforcement
- 📜 SBOM generation
- ⛔ OPA/Gatekeeper policy blocks

---

## ✅ Summary Table

| Area            | Tool/Practice                    |
|-----------------|----------------------------------|
| Static Code Scan| SonarQube                        |
| License & CVEs  | BlackDuck                        |
| Secrets         | GitHub Secrets / Vault / OIDC    |
| Build & Sign    | Docker + Cosign                  |
| Deploy          | GitHub Actions / ArgoCD / Helm   |
| Notification    | Slack, Jira                      |
| Governance      | Branch protection, policy-as-code|

