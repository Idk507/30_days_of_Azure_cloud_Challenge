Here's a comprehensive guide to **Azure Bicep and Terraform for Infrastructure as Code (IaC)** — from **zero to mastery**.

---

# **Azure Bicep & Terraform for IaC: Zero to Mastery**

---

## 🧱 **PART 1: Introduction to Infrastructure as Code (IaC)**

### ❖ What is IaC?

Infrastructure as Code allows you to **provision, manage, and version infrastructure using code** instead of manual processes. Benefits include:

* Automation
* Version control
* Repeatability
* Scalability
* Cost and time efficiency

---

## 🔹 **PART 2: Azure Bicep — From Zero to Mastery**

### ✅ What is Azure Bicep?

Azure Bicep is a **declarative domain-specific language (DSL)** for deploying Azure resources, designed to simplify **ARM templates**.

### ✅ Why Bicep?

* Cleaner syntax than JSON ARM templates
* Native support in Azure CLI and Visual Studio Code
* Built and maintained by Microsoft
* Easy integration with Azure DevOps and GitHub Actions

---

### ✅ Getting Started with Bicep

#### Prerequisites:

* Azure CLI (`az`)
* Bicep CLI (`az bicep install`)
* VS Code with Bicep extension

#### Create a Bicep File

```bicep
// main.bicep
resource myStorage 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: 'mystorageacct1234'
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {}
}
```

#### Deploy Bicep File

```bash
az deployment group create \
  --resource-group my-rg \
  --template-file main.bicep
```

---

### ✅ Bicep Modules & Parameters

```bicep
param location string = resourceGroup().location
param storageName string

resource stg 'Microsoft.Storage/storageAccounts@2022-09-01' = {
  name: storageName
  location: location
  sku: { name: 'Standard_LRS' }
  kind: 'StorageV2'
  properties: {}
}
```

To pass parameters:

```bash
az deployment group create \
  --resource-group my-rg \
  --template-file main.bicep \
  --parameters storageName='myuniquestorageacct'
```

---

### ✅ Bicep Features:

* **Loops & Conditions**
* **Modules** (reusable templates)
* **Outputs** for resource data
* **Type safety** and IntelliSense
* **Template validation**

---

## 🔹 **PART 3: Terraform on Azure — From Zero to Mastery**

### ✅ What is Terraform?

Terraform is an **open-source IaC tool** by HashiCorp that uses **HCL (HashiCorp Configuration Language)** to provision infrastructure across multiple cloud providers (Azure, AWS, GCP, etc.).

---

### ✅ Why Terraform?

* Multi-cloud support
* Large community and ecosystem
* Rich module support (Terraform Registry)
* Declarative and idempotent

---

### ✅ Getting Started with Terraform on Azure

#### Prerequisites:

* Azure CLI
* Terraform CLI (`terraform`)
* Azure Service Principal (for authentication)

---

### ✅ Simple Terraform Script

```hcl
# main.tf
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "my-rg"
  location = "East US"
}

resource "azurerm_storage_account" "example" {
  name                     = "mystorageacct1234"
  resource_group_name      = azurerm_resource_group.example.name
  location                 = azurerm_resource_group.example.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

#### Initialize, Plan, Apply

```bash
terraform init
terraform plan
terraform apply
```

---

### ✅ Terraform Key Concepts

| Concept    | Description                       |
| ---------- | --------------------------------- |
| `provider` | Defines cloud (e.g., Azure, AWS)  |
| `resource` | Defines infrastructure component  |
| `variable` | Inputs for reusable scripts       |
| `output`   | Outputs values (e.g., IP address) |
| `state`    | Tracks deployed resources         |
| `module`   | Reusable chunk of configuration   |

---

### ✅ Terraform State

* `.tfstate` file stores metadata of the infrastructure.
* Should be **stored remotely (Azure Storage Account)** for team use.

#### Example:

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "tfstate-rg"
    storage_account_name = "tfstateacct"
    container_name       = "tfstate"
    key                  = "terraform.tfstate"
  }
}
```

---

## 🔹 **PART 4: Bicep vs Terraform Comparison**

| Feature           | Bicep                    | Terraform                              |
| ----------------- | ------------------------ | -------------------------------------- |
| Language          | Bicep DSL                | HCL (HashiCorp Configuration Language) |
| Cloud Support     | Azure only               | Multi-cloud (Azure, AWS, GCP)          |
| Tooling           | Azure CLI, VS Code       | Terraform CLI, IDEs                    |
| State Management  | Handled by Azure         | Handled by Terraform (local/remote)    |
| Learning Curve    | Easier for Azure users   | Slightly steeper                       |
| Community Modules | Bicep registry (limited) | Terraform Registry (extensive)         |

---

## 🔹 **PART 5: CI/CD Integration**

### ✅ GitHub Actions for Bicep Deployment

```yaml
name: Deploy Bicep

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

    - name: Deploy Bicep Template
      run: |
        az deployment group create \
          --resource-group my-rg \
          --template-file main.bicep
```

---

### ✅ Azure DevOps Pipeline for Terraform

```yaml
trigger:
  branches:
    include:
    - main

pool:
  vmImage: 'ubuntu-latest'

steps:
- checkout: self

- task: TerraformInstaller@1
  inputs:
    terraformVersion: '1.6.0'

- script: terraform init
- script: terraform plan
- script: terraform apply -auto-approve
```

---

