
---

# 🧠 What Are Azure Functions?

**Azure Functions** is a **serverless compute service** provided by Microsoft Azure that allows you to run event-driven code without managing infrastructure.

You only focus on writing **business logic**. Azure handles provisioning, scaling, and patching.

---

## 📘 Why Use Azure Functions?

* **No infrastructure to manage**
* **Auto-scaling** based on demand
* **Pay-per-use** (pay only when function runs)
* **Event-driven** (triggered by HTTP, Queue, Blob, Timer, etc.)
* **Ideal for microservices, automation, APIs, and workflows**

---

# 🔧 Core Concepts of Azure Functions

## 1. **Function App**

> A **container for one or more Azure Functions** that share configuration, deployment settings, and scaling rules.

* Think of it as a **web app hosting environment** for functions.
* Contains:

  * Runtime version
  * Application settings
  * Storage account
  * Deployment slots (optional)

---

## 2. **Triggers**

> The **entry point** for function execution.

A trigger defines **how a function is invoked**. Each function **must have one and only one trigger**.

| Trigger Type  | Description                                        | Example Use Case                      |
| ------------- | -------------------------------------------------- | ------------------------------------- |
| HTTP Trigger  | Executes on HTTP requests                          | Build APIs, webhooks                  |
| Timer Trigger | Executes on a schedule (CRON expression)           | Scheduled cleanups, reports           |
| Blob Trigger  | Runs when a blob is added/modified in Blob Storage | Image processing, logging             |
| Queue Trigger | Runs when a message is added to a queue            | Order processing, notifications       |
| Event Grid    | Responds to system-wide events                     | Resource changes, alerts              |
| Service Bus   | Runs on messages from a Service Bus Queue/Topic    | Decoupled microservices communication |
| Cosmos DB     | Triggers on DB inserts or changes                  | Real-time analytics                   |

---

## 3. **Bindings**

> Connect your function to other services **without writing boilerplate code**.

### 🔹 Input Bindings

* Fetch external data before execution
* Example: Read from Blob, Cosmos DB, or Table Storage

### 🔹 Output Bindings

* Send results to external systems
* Example: Send email, write to Blob, update database

Example:

```csharp
[FunctionName("ProcessOrder")]
public static async Task Run(
    [QueueTrigger("orders")] string order,
    [Blob("processed-orders/{rand-guid}.json", FileAccess.Write)] Stream outputBlob,
    ILogger log)
```

---

## 4. **Hosting Plans**

| Plan                             | Description                            | Best Use Case                   |
| -------------------------------- | -------------------------------------- | ------------------------------- |
| **Consumption Plan**             | Auto-scales; billed per execution/time | Lightweight, infrequent tasks   |
| **Premium Plan**                 | Always warm, VNET support, autoscale   | High-performance APIs           |
| **Dedicated (App Service Plan)** | Use your own VMs                       | Long-running, predictable loads |
| **Elastic Premium**              | Combines scaling & VNET integration    | Secure and fast microservices   |

---

## 5. **Scaling**

Azure Functions scale **automatically**:

* Based on number of incoming events
* Can scale to **0** (no cost when idle on Consumption Plan)
* Up to 200 instances depending on quota

Premium and App Service Plans provide **pre-warmed instances** to avoid cold starts.

---

## 6. **Stateful Functions: Durable Functions**

> A framework built on top of Azure Functions for **orchestration** and **state management**.

### Types:

* **Orchestrator Function**: Defines the workflow
* **Activity Function**: Contains units of work
* **Entity Function**: Manages durable state

Use Cases:

* Order workflows
* Approval chains
* Fan-out/fan-in patterns

Example:

```python
@orchestration_function
def run_orchestrator(context):
    outputs = []
    outputs.append(yield context.call_activity('ActivityA', 'Data'))
    outputs.append(yield context.call_activity('ActivityB', 'Data'))
    return outputs
```

---

## 7. **CI/CD Integration**

Azure Functions can be integrated with:

* GitHub Actions
* Azure DevOps Pipelines
* Zip deploy, FTP, or Azure CLI

Typical GitHub Actions Example:

```yaml
name: Deploy Azure Function
on: [push]
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: Azure/functions-action@v1
      with:
        app-name: 'myfunctionapp'
        package: '.'
```

---

## 8. **Monitoring and Logging**

Azure Functions integrate with:

* **Azure Monitor** (metrics)
* **Application Insights** (logs, traces, live metrics)
* **Log Analytics**

Use this for:

* Exception tracking
* Cold start analysis
* Performance bottlenecks

---

## 9. **Security**

* Use **Azure Key Vault** to manage secrets (connection strings, tokens)
* Use **Managed Identity** to securely access resources (SQL, Blob)
* Protect HTTP triggers with:

  * Function Keys
  * OAuth2 with Azure AD
  * IP restrictions or API Management

---

## 10. **Local Development**

Tools:

* VS Code with Azure Functions extension
* Azure CLI or Core Tools
* Docker support for containerized function apps

Common commands:

```bash
func init MyFuncApp --worker-runtime node
func new --name HttpTrigger1 --template "HTTP trigger"
func start
```

---

## 📈 When to Use Azure Functions?

✅ Ideal for:

* APIs
* Event handling (Blob upload, Queue message)
* Scheduled jobs
* Serverless backends
* Lightweight data processing

❌ Avoid for:

* Long-running workflows (>10 min on Consumption Plan)
* Apps needing full control over the OS
* Real-time low-latency workloads with high IOPS

---

## 📦 Real-Time Example: File Upload + Processing

**Scenario**:

* User uploads a file to Blob Storage
* A Blob Triggered Function processes it
* Stores metadata in Cosmos DB
* Sends a success email

**Flow**:

1. Blob Storage upload triggers function
2. Function reads content, parses it
3. Saves to Cosmos DB via output binding
4. Sends email via SendGrid output binding

---

