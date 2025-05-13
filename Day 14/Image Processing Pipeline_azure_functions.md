

---

## 🧩 **Real-Time Use Case: Image Processing Pipeline**

**Scenario**:
You're building an automated system where users upload images to a website, and the system:

1. Stores the image in Azure Blob Storage
2. Resizes the image (thumbnail creation)
3. Saves metadata to Cosmos DB
4. Sends an email notification once done

All of this is **automated using Azure Functions**.

---

## ✅ **Tech Stack Used**

| Component           | Purpose                                    |
| ------------------- | ------------------------------------------ |
| Azure Blob Storage  | Store uploaded images                      |
| Azure Function      | Triggered on upload, processes the image   |
| Azure Cosmos DB     | Store metadata (filename, user, timestamp) |
| SendGrid API        | Sends email notifications                  |
| Azure Key Vault     | Store API keys securely                    |
| Azure Monitor       | Logs and metrics                           |
| Azure DevOps/GitHub | CI/CD pipeline for deployment              |

---

## 🧱 **Architecture Diagram**

```
Client --> Uploads Image --> Blob Storage
                            ↓ (Blob Trigger)
                      Azure Function App
                    ┌──────────────┬──────────────┐
                    ↓              ↓              ↓
         Image Resize Function   Cosmos DB       SendGrid
         (via Durable Functions) (Metadata)      (Email)
```

---

# 🛠️ Step-by-Step Implementation

---

## 🔹 Step 1: **Create a Function App**

You can create a Function App using Azure Portal, Azure CLI, or Bicep/Terraform.

**Using CLI:**

```bash
az functionapp create \
  --resource-group myResourceGroup \
  --consumption-plan-location eastus \
  --runtime python \
  --functions-version 4 \
  --name myImageFunctionApp \
  --storage-account mystorageaccount
```

---

## 🔹 Step 2: **Configure Blob Trigger Function**

This function is invoked when an image is uploaded to Blob Storage.

**Example in Python:**

```python
import logging

def main(blob: bytes, filename: str):
    logging.info(f"Processing uploaded image: {filename}")
    # Resize logic or call downstream function
```

> Configure `function.json`:

```json
{
  "bindings": [
    {
      "type": "blobTrigger",
      "name": "blob",
      "direction": "in",
      "path": "images/{name}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

---

## 🔹 Step 3: **Process the Image (Resizing)**

You can create a separate **Activity Function** or write it inline using `Pillow` (Python image library).

```python
from PIL import Image
import io

def resize_image(image_data):
    img = Image.open(io.BytesIO(image_data))
    img.thumbnail((100, 100))
    output = io.BytesIO()
    img.save(output, format='JPEG')
    return output.getvalue()
```

---

## 🔹 Step 4: **Store Metadata in Cosmos DB**

Add an **Output Binding** or manually use SDK to write to Cosmos DB.

```python
def store_metadata(image_name, user_id):
    document = {
        "id": str(uuid.uuid4()),
        "filename": image_name,
        "user": user_id,
        "timestamp": datetime.utcnow().isoformat()
    }
    container.upsert_item(document)
```

---

## 🔹 Step 5: **Send Email via SendGrid**

Use the **SendGrid output binding** or call the REST API using requests.

```python
import requests

def send_email(to_email, subject, message):
    headers = {
        "Authorization": f"Bearer {SENDGRID_API_KEY}",
        "Content-Type": "application/json"
    }
    data = {
        "personalizations": [{"to": [{"email": to_email}]}],
        "from": {"email": "noreply@yourapp.com"},
        "subject": subject,
        "content": [{"type": "text/plain", "value": message}]
    }
    requests.post("https://api.sendgrid.com/v3/mail/send", headers=headers, json=data)
```

---

## 🔹 Step 6: **Secure with Key Vault + Managed Identity**

* Store the `SendGrid API Key`, Cosmos DB secrets in Azure Key Vault
* Configure **Managed Identity** for the Function App to access secrets securely

```bash
az keyvault secret set --vault-name myvault --name "SendGridKey" --value "SG.xxx"
```

In your function:

```python
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

credential = DefaultAzureCredential()
client = SecretClient(vault_url="https://myvault.vault.azure.net/", credential=credential)
SENDGRID_API_KEY = client.get_secret("SendGridKey").value
```

---

## 🔹 Step 7: **Setup CI/CD for Deployment**

Use **GitHub Actions** or **Azure DevOps** to automatically deploy on push.

**Example GitHub Workflow:**

```yaml
name: Deploy to Azure Function App

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - uses: Azure/functions-action@v1
      with:
        app-name: myImageFunctionApp
        package: '.'
        publish-profile: ${{ secrets.AZURE_PUBLISH_PROFILE }}
```

---

## 🔹 Step 8: **Monitor with Application Insights**

* Go to Azure Portal → Function App → Application Insights
* View logs, traces, exceptions, performance metrics
* Use `ILogger` in C# or `logging` in Python

```python
import logging
logging.info("Function started...")
logging.error("Error occurred")
```

---

## 🔹 Step 9: **Autoscaling and Cold Start**

* On the **Consumption Plan**, your function can scale from 0 to hundreds of instances.
* For mission-critical apps, use **Premium Plan** to avoid cold starts and enable **VNET** support.

---

# 🏁 Final Outcome

* User uploads an image
* Trigger function runs and:

  * Resizes image
  * Stores metadata
  * Sends email
* Logs and performance can be monitored
* All code is version-controlled and deployed automatically

---


