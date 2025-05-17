
---

# ✅ Azure Architect Project: **Enterprise Web Application Architecture**

---

## 🔰 PROJECT OVERVIEW

We will build a **scalable web application infrastructure on Azure**, suitable for real-world enterprise applications. This project includes:

* Frontend hosted in **App Service**
* Backend via **Azure Functions**
* **Azure SQL** for database
* **Blob Storage** for file storage
* **Virtual Network** for secure communication
* **Application Gateway + WAF** for traffic routing and security
* **Monitoring and Logging**
* **IaC with Bicep or Terraform**
* CI/CD with **GitHub Actions**

---

## 🧱 Step 1: Understand the Architecture (Big Picture)

```
User ---> Azure DNS --> App Gateway (WAF)
                        |       |
                    App Service (Frontend)
                        |
                 Azure Functions (API)
                        |
        -------------------------------
        |                             |
     Azure SQL                 Blob Storage
        |
    Log Analytics & Azure Monitor
```

---

## 🛠️ Step 2: Tools & Services Used

| Component        | Azure Service                   |
| ---------------- | ------------------------------- |
| Frontend         | Azure App Service               |
| Backend API      | Azure Functions                 |
| Database         | Azure SQL                       |
| File Storage     | Azure Blob Storage              |
| Load Balancer    | Azure Application Gateway + WAF |
| Network Security | Azure VNet, NSG                 |
| Monitoring       | Azure Monitor, Log Analytics    |
| IaC              | Azure Bicep / Terraform         |
| CI/CD            | GitHub Actions                  |

---

## 🚀 Step 3: Create the Resources Step-by-Step

### ✅ 3.1 Create Resource Group

```bash
az group create --name webapp-rg --location eastus
```

---

### ✅ 3.2 Create Virtual Network (VNet)

```bash
az network vnet create \
  --resource-group webapp-rg \
  --name vnet-app \
  --address-prefix 10.0.0.0/16 \
  --subnet-name web-subnet \
  --subnet-prefix 10.0.1.0/24
```

---

### ✅ 3.3 Deploy App Service for Frontend

```bash
az appservice plan create \
  --name frontend-plan \
  --resource-group webapp-rg \
  --sku B1

az webapp create \
  --resource-group webapp-rg \
  --plan frontend-plan \
  --name myfrontendapp1234 \
  --runtime "NODE|18-lts"
```

---

### ✅ 3.4 Deploy Azure Functions for Backend API

```bash
az functionapp create \
  --resource-group webapp-rg \
  --consumption-plan-location eastus \
  --runtime node \
  --functions-version 4 \
  --name backendapi1234 \
  --storage-account mystoragefunc123
```

---

### ✅ 3.5 Provision Azure SQL Database

```bash
az sql server create \
  --name sqlserverweb123 \
  --resource-group webapp-rg \
  --location eastus \
  --admin-user sqladmin \
  --admin-password MyP@ssword123

az sql db create \
  --resource-group webapp-rg \
  --server sqlserverweb123 \
  --name webdb \
  --service-objective S0
```

---

### ✅ 3.6 Enable Blob Storage

```bash
az storage account create \
  --name mystorageblob123 \
  --resource-group webapp-rg \
  --location eastus \
  --sku Standard_LRS \
  --kind StorageV2
```

---

### ✅ 3.7 Add Application Gateway with WAF

```bash
az network public-ip create \
  --resource-group webapp-rg \
  --name appgw-ip \
  --sku Standard

az network application-gateway create \
  --name appGateway \
  --location eastus \
  --resource-group webapp-rg \
  --sku WAF_v2 \
  --capacity 2 \
  --frontend-port 80 \
  --http-settings-cookie-based-affinity Enabled \
  --public-ip-address appgw-ip \
  --vnet-name vnet-app \
  --subnet web-subnet
```

---

### ✅ 3.8 Monitoring with Azure Monitor

```bash
az monitor log-analytics workspace create \
  --resource-group webapp-rg \
  --workspace-name webapp-logws

az monitor diagnostic-settings create \
  --name log-settings \
  --resource /subscriptions/<sub-id>/resourceGroups/webapp-rg/providers/Microsoft.Web/sites/myfrontendapp1234 \
  --workspace webapp-logws \
  --logs '[{"category": "AppServiceConsoleLogs", "enabled": true}]'
```

---

## ⚙️ Step 4: Infrastructure as Code (IaC)

You can use either **Bicep** or **Terraform** to automate all the above.

Would you like a full Bicep or Terraform script for this project? (Let me know and I will generate one for you.)

---

## 🔁 Step 5: CI/CD with GitHub Actions

### `.github/workflows/deploy-webapp.yml`

```yaml
name: Azure Web App CI/CD

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    - name: Login to Azure
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Deploy to Web App
      uses: azure/webapps-deploy@v2
      with:
        app-name: myfrontendapp1234
        package: .
```

---

## 🎯 Step 6: Optional Add-ons for Mastery

* Add **Azure Key Vault** for secrets
* Add **Azure Front Door** for global traffic distribution
* Use **Managed Identity** to secure access
* Create **Private Endpoints** for secure DB/Storage
* Add **Azure Policy** for compliance

---

## 🧠 Summary

By the end of this project, you will learn:

✅ How to architect a secure, scalable Azure application
✅ How to deploy infrastructure using CLI and IaC
✅ How to integrate CI/CD with GitHub Actions
✅ How to monitor and secure your application

---

