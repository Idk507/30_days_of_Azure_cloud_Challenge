# **Hands-on Lab: Implementing Azure IAM (Step-by-Step Guide) 🚀**  

This hands-on lab will guide you through the **practical implementation of Azure IAM** using **Azure Portal** and **Azure CLI**.

---

## **🎯 Lab Goals**
By the end of this lab, you will:
✅ Create an **Azure Active Directory (Azure AD) User**  
✅ Assign **Role-Based Access Control (RBAC) Roles**  
✅ Configure **Conditional Access Policies**  
✅ Enable **Multi-Factor Authentication (MFA)**  
✅ Monitor IAM activity using **Azure Logs**  

---

## **🔧 Prerequisites**
- An **Azure Subscription** ([Sign up for a free trial](https://azure.microsoft.com/en-us/free/))
- **Azure CLI installed** ([Install Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli))
- Administrator permissions for IAM configuration  

---

## **🛠️ Step 1: Create a New User in Azure AD**
📌 We will create a user and later assign permissions.  

### **🔹 Method 1: Using Azure Portal**
1. Go to **Azure Portal** → [https://portal.azure.com](https://portal.azure.com)  
2. Navigate to **Azure Active Directory** → Click **Users**.  
3. Click **+ New User** → Choose **Create User**.  
4. Enter:  
   - **Username:** `john.doe@yourdomain.onmicrosoft.com`  
   - **Name:** `John Doe`  
   - **Password:** Generate a **temporary password**  
5. Click **Create**.  

✅ **User Created!** John Doe can now sign in to Azure.  

---

### **🔹 Method 2: Using Azure CLI**
```bash
az ad user create \
  --display-name "John Doe" \
  --user-principal-name "john.doe@yourdomain.onmicrosoft.com" \
  --password "P@ssw0rd123!"
```
✅ **User Created via CLI!**  

---

## **🛠️ Step 2: Assign RBAC Role to the User**
📌 We will assign **Reader** access to John Doe at the **Resource Group level**.  

### **🔹 Method 1: Using Azure Portal**
1. Go to **Azure Portal** → **Resource Groups**.  
2. Select an **existing Resource Group** or create a new one.  
3. Click **Access control (IAM)** → **+ Add role assignment**.  
4. Select:  
   - **Role:** `Reader`  
   - **Assign access to:** `User`  
   - **Select User:** `John Doe`  
5. Click **Review + Assign**.  

✅ John Doe can now **view** resources but **cannot modify them**.  

---

### **🔹 Method 2: Using Azure CLI**
```bash
az role assignment create \
  --assignee john.doe@yourdomain.onmicrosoft.com \
  --role "Reader" \
  --scope /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>
```
✅ **RBAC Role Assigned!**  

---

## **🛠️ Step 3: Enable Multi-Factor Authentication (MFA)**
📌 We will enforce **MFA for all users** in a specific role.  

### **🔹 Using Azure Portal**
1. Go to **Azure Active Directory** → **Security** → **Conditional Access**.  
2. Click **+ New Policy** → Name it **"Enforce MFA for All Admins"**.  
3. Under **Assignments**, select:  
   - **Users:** Choose **All Admin Users**  
   - **Cloud Apps:** Select **All Cloud Apps**  
4. Under **Access Controls**, select:  
   - **Grant Access** → Check **Require Multi-Factor Authentication**  
5. Click **Create Policy**.  

✅ Now, all Admins must verify their identity with **MFA**.  

---

## **🛠️ Step 4: Monitor IAM Activity Using Azure Logs**
📌 We will track all **role assignments** using **Azure Monitor Logs**.  

### **🔹 Using Azure Portal**
1. Go to **Azure Monitor** → Click **Logs**.  
2. Run the following query in **Log Analytics**:
   ```kusto
   AuditLogs
   | where ActivityDisplayName == "Role assignment created"
   | project TimeGenerated, ActivityDisplayName, InitiatedBy, TargetResources
   ```
✅ This will show all **new role assignments** in your Azure environment.  

---

## **🛠️ Step 5: Remove IAM Role and User (Cleanup)**
📌 To remove the IAM role assignment and delete the user.

### **🔹 Remove IAM Role Assignment (Using CLI)**
```bash
az role assignment delete \
  --assignee john.doe@yourdomain.onmicrosoft.com \
  --role "Reader" \
  --scope /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>
```

### **🔹 Delete the User (Using CLI)**
```bash
az ad user delete --id "john.doe@yourdomain.onmicrosoft.com"
```
✅ IAM role removed and user deleted!  

---

## **🎯 Summary of What We Did**
✅ **Created a new Azure AD user**  
✅ **Assigned RBAC role (Reader)**  
✅ **Enforced MFA using Conditional Access**  
✅ **Monitored IAM activity with Azure Logs**  
✅ **Removed IAM role & deleted the user**  

**You have successfully implemented Azure IAM!** 🚀  
