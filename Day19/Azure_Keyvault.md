Here's a **comprehensive and professional guide to Azure Key Vault**, covering every core concept **from zero to mastery**, suitable for cloud architects, security engineers, and developers.

---

## 🔐 What is Azure Key Vault?

**Azure Key Vault** is a **cloud service** that enables **secure storage and access to secrets**, such as:

* **Secrets** – API keys, passwords, connection strings
* **Keys** – Cryptographic keys used for encryption/decryption, signing, etc.
* **Certificates** – SSL/TLS certificates with lifecycle management

It's designed to **centralize secret management**, **secure sensitive data**, and **integrate seamlessly with Azure services** and DevOps pipelines.

---

## 🧱 Core Components of Azure Key Vault

| Component              | Description                                                         |
| ---------------------- | ------------------------------------------------------------------- |
| **Vault**              | A secure container that stores keys, secrets, and certificates      |
| **Secrets**            | Secure string values (e.g., passwords, tokens)                      |
| **Keys**               | RSA and EC keys for cryptographic operations                        |
| **Certificates**       | SSL/TLS certs managed via Key Vault or integrated with CA providers |
| **Access Policies**    | Define who/what can access the contents of the vault                |
| **Managed Identities** | Allows Azure services to authenticate to Key Vault without secrets  |

---

## 🛡️ Use Cases for Azure Key Vault

| Use Case                      | How Key Vault Helps                                        |
| ----------------------------- | ---------------------------------------------------------- |
| Store sensitive configuration | Avoid storing secrets in app code or config files          |
| Key management for encryption | Create, rotate, and revoke cryptographic keys securely     |
| TLS certificate management    | Auto-renew public SSL certificates for App Services, etc.  |
| DevOps secret injection       | Integrate with GitHub Actions, Azure DevOps, Terraform     |
| Disk encryption               | Integrate with Azure Disk Encryption for VM OS/Data disks  |
| HSM-backed security           | Use FIPS 140-2 Level 2 certified Hardware Security Modules |

---

## 🔐 Types of Secrets and Operations

### 1. **Secrets**

* Plain text values (e.g., passwords, tokens)
* Stored as key-value pairs
* Versioned and can be set with expiry

**Operations:**

* `SetSecret`, `GetSecret`, `DeleteSecret`, `ListSecrets`

---

### 2. **Keys**

* RSA & Elliptic Curve (EC) keys
* Used for:

  * Encryption/Decryption
  * Signing/Verifying
* Optional **Hardware Security Module (HSM)** protection

**Operations:**

* `Encrypt`, `Decrypt`, `WrapKey`, `UnwrapKey`, `Sign`, `Verify`

---

### 3. **Certificates**

* Can import, generate, or renew certificates
* Integrated with CA providers like DigiCert
* Supports certificate policies (auto-renewal, validity, etc.)

**Operations:**

* `CreateCertificate`, `GetCertificate`, `ImportCertificate`

---

## 🔒 Access Control in Azure Key Vault

### 1. **Access Policies** (Vault-level)

* Legacy model
* Specify **permissions per object type (secret/key/cert)** for each user/service

### 2. **Azure RBAC** (Recommended)

* Granular role assignments at subscription/resource level
* Use predefined roles like:

  * **Key Vault Reader**
  * **Key Vault Secrets User**
  * **Key Vault Administrator**

### 3. **Authentication**

* **Managed Identity (MSI)** for Azure resources
* **Service Principals** for apps and pipelines
* **User-assigned or System-assigned MSI**

---

## 🔧 How to Use Azure Key Vault (Step-by-Step)

### **Step 1: Create a Key Vault**

```bash
az keyvault create --name myKeyVault --resource-group myRG --location eastus
```

### **Step 2: Add a Secret**

```bash
az keyvault secret set --vault-name myKeyVault --name "DbPassword" --value "SuperSecure123"
```

### **Step 3: Get the Secret in Code**

Example using Python SDK:

```python
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

credential = DefaultAzureCredential()
client = SecretClient(vault_url="https://myKeyVault.vault.azure.net/", credential=credential)
secret = client.get_secret("DbPassword")
print(secret.value)
```

### **Step 4: Access from Azure App Service**

* Enable **Managed Identity** for the App Service
* Grant the identity `Key Vault Secrets User` role
* Reference secret in App Settings using:

  ```
  @Microsoft.KeyVault(SecretUri=https://myKeyVault.vault.azure.net/secrets/DbPassword/...)
  ```

---

## 🔁 Key Rotation & Certificate Auto-Renewal

### **Key Rotation**

* Automatic key rotation policies can be set for keys
* Rotate manually using CLI/SDK when needed

### **Certificate Auto-Renewal**

* For App Services or APIs using custom domains
* Integrate with CA providers for automatic issuance and renewal

---

## 🔐 Security Best Practices

* Use **Azure RBAC** over legacy Access Policies
* Enable **soft-delete** and **purge protection**
* Use **Private Endpoints** to restrict network access
* Enable **Diagnostic Settings** to log access operations
* Set **expiration dates** for all secrets
* Rotate keys and secrets regularly (via Key Rotation Policies or Azure Automation)

---

## 🌍 Integration with Other Azure Services

| Azure Service                  | Integration with Key Vault                                |
| ------------------------------ | --------------------------------------------------------- |
| Azure App Service              | Store secrets & reference them in configuration securely  |
| Azure Functions                | Fetch secrets at runtime using managed identity           |
| Azure DevOps                   | Secure pipeline secrets via Key Vault task                |
| GitHub Actions                 | Use Azure Login and Key Vault action for secret injection |
| Azure Disk Encryption          | Use customer-managed keys stored in Key Vault             |
| Azure Kubernetes Service (AKS) | Use CSI driver to mount secrets as volumes                |

---

## 📈 Monitoring & Auditing

* Enable **Azure Monitor** logs for Key Vault access
* View activity logs in **Azure Activity Log**
* Export logs to **Log Analytics**, **Event Hubs**, or **Storage**
* Monitor:

  * Unauthorized access attempts
  * Secret expiry
  * Access patterns

---

## 🧠 Real-World Example

### Scenario: Secure CI/CD Secrets in Azure DevOps

1. Create a Key Vault and store:

   * GitHub PAT
   * SQL Admin Password
2. Assign **Azure DevOps** service principal access to secrets
3. Use Key Vault task in Azure Pipeline YAML:

```yaml
- task: AzureKeyVault@2
  inputs:
    azureSubscription: 'MyConnection'
    KeyVaultName: 'myKeyVault'
    SecretsFilter: '*'
```

4. Use secrets in later tasks with `$(DbPassword)`, etc.

---

## 🧾 Summary

| Feature                       | Support                  |
| ----------------------------- | ------------------------ |
| Secret management             | ✅                        |
| Key operations (sign/encrypt) | ✅                        |
| HSM-backed keys               | ✅ (premium tier)         |
| Certificate lifecycle         | ✅                        |
| Role-based access (RBAC)      | ✅                        |
| Logging & auditing            | ✅                        |
| Private networking            | ✅ (via Private Endpoint) |

---

