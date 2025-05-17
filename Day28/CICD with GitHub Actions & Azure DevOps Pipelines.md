
---

# **CI/CD with GitHub Actions & Azure DevOps Pipelines — From Zero to Mastery**

---

## 🔹 **PART 1: Understanding CI/CD Concepts**

### ❖ What is CI/CD?

* **Continuous Integration (CI):** Automating the integration of code changes from multiple contributors into a shared repository several times a day.
* **Continuous Delivery (CD):** Automatically deploying validated code to test or production environments.

### ❖ Benefits of CI/CD:

* Faster releases
* Fewer bugs
* Automated workflows
* Better collaboration
* Shorter feedback cycles

---

## 🔹 **PART 2: GitHub Actions — Overview & Mastery**

### ✅ What is GitHub Actions?

GitHub Actions lets you **automate workflows directly in your GitHub repo**. It's event-driven and YAML-based.

### ✅ Core Components:

| Term     | Description                                             |
| -------- | ------------------------------------------------------- |
| Workflow | A YAML file that defines automation (CI/CD steps)       |
| Job      | A unit of work that runs on a VM or container           |
| Step     | A command/script that runs inside a job                 |
| Action   | A reusable script (Docker or JavaScript-based)          |
| Runner   | Server to execute workflows (GitHub-hosted/self-hosted) |

---

### ✅ GitHub Actions Syntax (Workflow Example)

```yaml
# .github/workflows/ci.yml
name: Node.js CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test
```

---

### ✅ Use Cases of GitHub Actions

| Task         | Example                                              |
| ------------ | ---------------------------------------------------- |
| Build/Test   | Run unit tests on code push                          |
| Linting      | Auto-run ESLint or Prettier                          |
| Deployment   | Deploy to Azure App Service or Azure Static Web Apps |
| Docker       | Build and push images to ACR or DockerHub            |
| Notification | Post results to Slack, Teams, or email               |

---

### ✅ Azure Deployment with GitHub Actions

```yaml
name: Deploy to Azure

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Azure Login
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}

      - name: Deploy to Web App
        uses: azure/webapps-deploy@v2
        with:
          app-name: 'my-azure-app'
          package: '.'
```

---

## 🔹 **PART 3: Azure DevOps Pipelines — Overview & Mastery**

### ✅ What is Azure DevOps Pipeline?

Azure Pipelines provide **CI/CD automation** for any project hosted in **Azure Repos**, **GitHub**, or other Git providers.

---

### ✅ Pipeline Types:

| Type           | Description                  |
| -------------- | ---------------------------- |
| Classic        | GUI-based (drag-and-drop)    |
| YAML Pipelines | Code-as-infrastructure style |

---

### ✅ Basic Azure Pipeline YAML Example

```yaml
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: UseNode@1
  inputs:
    version: '18.x'

- script: npm install
  displayName: 'Install dependencies'

- script: npm run build
  displayName: 'Build the app'

- script: npm test
  displayName: 'Run tests'
```

---

### ✅ Deployment Using Azure DevOps

#### Example: Deploy to Azure App Service

```yaml
- task: AzureWebApp@1
  inputs:
    azureSubscription: '<Azure-Service-Connection>'
    appName: 'my-azure-app'
    package: '$(System.DefaultWorkingDirectory)/**/*.zip'
```

---

## 🔹 **PART 4: GitHub Actions vs Azure DevOps Pipelines**

| Feature            | GitHub Actions                  | Azure DevOps Pipelines                 |
| ------------------ | ------------------------------- | -------------------------------------- |
| Integration        | Native with GitHub              | Native with Azure Repos                |
| Pricing            | Free tier with limits           | Free tier with Microsoft-hosted agents |
| Flexibility        | Highly flexible and composable  | Advanced GUI + YAML                    |
| Secrets Management | GitHub Secrets                  | Azure Key Vault, Secure Files          |
| Marketplace        | 10,000+ Actions                 | Hundreds of tasks/extensions           |
| Environments       | Supports approvals & deployment | Supports approvals, gates              |

---

## 🔹 **PART 5: Mastery Workflow (30-Day Learning Plan)**

| Day   | Focus                           | Goal                                              |
| ----- | ------------------------------- | ------------------------------------------------- |
| 1–3   | GitHub & Azure DevOps Setup     | Create repos, understand structure                |
| 4–6   | Write simple GitHub workflows   | Build + test app (CI)                             |
| 7–10  | Add deployments to Azure        | Use Web Apps or Static Web Apps with secrets      |
| 11–13 | Explore Azure Pipelines YAML    | Build & deploy Node, Python, .NET                 |
| 14–16 | Use Environments & Secrets      | Add security to deployment pipelines              |
| 17–20 | Setup Staging → Production flow | Use approvals or gates in DevOps                  |
| 21–24 | Containerize + Push to ACR      | Use Docker + ACR deploy automation                |
| 25–27 | Integrate Monitoring            | Log diagnostics with App Insights / GitHub status |
| 28–30 | Build Capstone Project          | End-to-end CI/CD pipeline on GitHub & DevOps      |

---

## 🔹 **PART 6: Best Practices**

* Use **branch filters** (`main`, `dev`) to separate staging/prod environments
* Use **secrets and encrypted tokens**
* Keep pipelines **modular and reusable**
* Add **tests and linting** as part of CI
* Enable **status checks** before merge
* Integrate **manual approvals** for production
* Monitor deployments with **Azure Monitor or GitHub Checks**

---

## 🔹 **PART 7: Useful Tools & Resources**

### GitHub Actions

* [Actions Marketplace](https://github.com/marketplace/actions)
* [Official Docs](https://docs.github.com/en/actions)

### Azure DevOps

* [YAML schema reference](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema)
* [Azure Pipelines tasks](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/)


