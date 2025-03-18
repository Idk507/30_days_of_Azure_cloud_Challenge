## **Step-by-Step Guide to Deploy Azure Bastion in the Azure Portal** 🚀  

This guide will walk you through **deploying Azure Bastion** and **securely accessing an Azure Virtual Machine (VM) without exposing it to the public internet**.  

---

## **🔹 Prerequisites**
Before you start, make sure you have:  
✅ An **Azure Subscription**.  
✅ A **Virtual Network (VNet)** with a subnet for Bastion.  
✅ A **Virtual Machine (VM)** deployed in the same VNet.  

---

## **🔹 Step 1: Create a Virtual Network (VNet)**
If you don’t already have a VNet, follow these steps:

### **1️⃣ Go to Azure Portal & Create a VNet**
1. Sign in to the **Azure Portal** → Search **Virtual Network** → Click **Create**.
2. Enter **VNet Name** (e.g., `MyVNet`).
3. Choose a **Region** where your VM is deployed.
4. Click **Next: IP Addresses**.

### **2️⃣ Define Address Space**
1. Under **IPv4 address space**, enter a range like `10.0.0.0/16`.
2. Click **+ Add subnet**.
   - Name: `MySubnet`
   - Subnet Address Range: `10.0.1.0/24`
3. Click **Review + Create** → **Create**.

---

## **🔹 Step 2: Create the Bastion Subnet**
Azure Bastion requires a specific subnet named `AzureBastionSubnet`.

### **1️⃣ Add Azure Bastion Subnet**
1. Go to **Virtual Network** → Select your **VNet (`MyVNet`)**.
2. Click **Subnets** → **+ Add Subnet**.
3. Set the **Subnet Name**: `AzureBastionSubnet` (this name is mandatory).
4. Set **Subnet Address Range**: At least **/26** (e.g., `10.0.2.0/26`).
5. Click **Save**.

✅ The subnet is now ready for Bastion deployment.

---

## **🔹 Step 3: Deploy Azure Bastion**
### **1️⃣ Create a Bastion Host**
1. Go to **Azure Portal** → Search **Bastion** → Click **Create**.
2. Enter:
   - **Resource Group**: (Select an existing or create a new one).
   - **Name**: `MyBastion`.
   - **Region**: (Same as your VNet & VM).
   - **Virtual Network**: Select `MyVNet`.
   - **Subnet**: Select `AzureBastionSubnet`.

### **2️⃣ Configure Public IP**
1. Click **Create new** (to create a new Public IP for Bastion).
2. Name the Public IP: `Bastion-Public-IP`.
3. **SKU**: `Standard` (recommended for security).
4. Click **Review + Create** → **Create**.

✅ Azure Bastion will now deploy. It may take **5-10 minutes**.

---

## **🔹 Step 4: Connect to a VM Using Bastion**
### **1️⃣ Go to the VM**
1. Navigate to **Virtual Machines** → Select your VM.
2. Click **Connect** → Select **Bastion**.

### **2️⃣ Access the VM Securely**
1. Enter the **VM username & password**.
2. Click **Connect**.
3. The VM will open in your **browser (no RDP/SSH client needed)!** 🎉

---

## **🔹 Step 5: (Optional) Enable Native RDP/SSH Clients**
If you want to connect using **Remote Desktop (RDP) or SSH clients** like PuTTY, you must upgrade to **Azure Bastion Premium SKU**.

### **Upgrade to Standard/Premium**
1. Go to **Bastion** → Select **Configuration**.
2. Choose **Standard** or **Premium** SKU.
3. Enable **Native Client Support**.
4. Click **Save**.

✅ Now, you can connect using **RDP (Remote Desktop) or SSH** directly from your local machine.

---

## **🔹 Summary of What We Did**
✅ Created a **Virtual Network (VNet)**.  
✅ Added a **subnet for Azure Bastion**.  
✅ Deployed **Azure Bastion**.  
✅ Connected to a **VM securely without a public IP**.  

🔒 **Your VM is now securely accessible without exposing it to the internet!** 🎉

---

1️⃣ Create Virtual Network (VNet)
   └── Configure CIDR and Subnets
       └── Add AzureBastionSubnet (Min /26)
           └── Ensure Subnet Name is 'AzureBastionSubnet'
               └── Assign IP Range for Subnet
               
2️⃣ Deploy Azure Bastion
   └── Go to Azure Portal > Create Bastion
       └── Select Virtual Network & AzureBastionSubnet
           └── Assign Public IP to Bastion
               └── Enable Required NSG Rules

3️⃣ Connect to VM via Bastion
   └── Navigate to VM > Click 'Connect'
       └── Choose 'Bastion' as Connection Method
           └── Authenticate with VM Credentials
               └── Securely Access VM via Browser (No RDP/SSH Needed)
