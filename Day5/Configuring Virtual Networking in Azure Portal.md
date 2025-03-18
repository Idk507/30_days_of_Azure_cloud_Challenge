### **Implementation Guide: Configuring Virtual Networking in Azure Portal**  

This guide walks you through setting up a **Virtual Network (VNet)**, **Subnets**, **NSGs**, **ASGs**, and **Route Tables** in **Azure Portal**.

---

## **Step 1: Create a Virtual Network (VNet)**  
### **1.1 Navigate to Virtual Networks**  
1. Sign in to **Azure Portal** ([portal.azure.com](https://portal.azure.com)).  
2. In the **Search bar**, type **"Virtual Network"** and select it.  
3. Click **+ Create** to start the process.  

### **1.2 Configure the VNet**  
1. **Subscription**: Select your **Azure Subscription**.  
2. **Resource Group**: Click **Create New** (or use an existing one).  
3. **Name**: Enter a name (e.g., `MyVNet`).  
4. **Region**: Choose a location (e.g., `East US`).  
5. **IPv4 Address Space**: Define the address range using CIDR notation (e.g., `10.0.0.0/16`).  
6. Click **Next: Security >**.  

### **1.3 Configure Security Settings**  
- Leave default settings or enable **DDoS Protection** and **Firewall** as needed.  
- Click **Next: Tags >** *(optional)*.  
- Click **Review + Create**, then **Create**.  

---

## **Step 2: Create and Configure Subnets**  
1. Open your **Virtual Network** (`MyVNet`).  
2. Click **Subnets** (on the left menu).  
3. Click **+ Subnet** to create a new subnet.  
4. Enter details:  
   - **Name**: e.g., `WebSubnet`.  
   - **Subnet Address Range**: e.g., `10.0.1.0/24`.  
   - **Network Security Group (NSG)**: None *(we will create it separately)*.  
5. Click **Add**.  
6. Repeat for other subnets (e.g., `AppSubnet`, `DBSubnet`).  

---

## **Step 3: Create a Network Security Group (NSG)**
### **3.1 Create an NSG**
1. Search for **Network Security Groups** in the Azure Portal.  
2. Click **+ Create**.  
3. Choose your **Subscription**, **Resource Group**, and enter **Name** (`MyNSG`).  
4. Select **Region** (must match VNet).  
5. Click **Review + Create**, then **Create**.  

### **3.2 Configure NSG Rules**
1. Open **MyNSG**.  
2. Click **Inbound Security Rules** → **+ Add Rule**.  
3. Configure:  
   - **Name**: `Allow-HTTP`.  
   - **Priority**: `100`.  
   - **Source**: `Any`.  
   - **Destination**: `Any` (or select Subnet).  
   - **Protocol**: `TCP`.  
   - **Port**: `80`.  
   - **Action**: `Allow`.  
4. Click **Add**.  
5. Repeat for **other rules** (e.g., Allow SSH on port `22`, Allow RDP on port `3389`).  

### **3.3 Associate NSG with a Subnet**
1. Open **MyNSG**.  
2. Click **Subnets** → **Associate**.  
3. Select your **VNet** (`MyVNet`).  
4. Choose a **Subnet** (e.g., `WebSubnet`).  
5. Click **OK**.  

---

## **Step 4: Configure Route Table**
### **4.1 Create a Route Table**
1. Search for **Route Tables** in the Azure Portal.  
2. Click **+ Create**.  
3. Enter:  
   - **Name**: `MyRouteTable`.  
   - **Subscription & Resource Group**: Choose the same as VNet.  
   - **Region**: Match the VNet region.  
4. Click **Review + Create**, then **Create**.  

### **4.2 Add a Custom Route**
1. Open **MyRouteTable**.  
2. Click **Routes** → **+ Add Route**.  
3. Enter:  
   - **Name**: `Force-Internet-Traffic`.  
   - **Address Prefix**: `0.0.0.0/0` (for internet).  
   - **Next Hop Type**: `Internet`.  
4. Click **Add**.  

### **4.3 Associate Route Table to a Subnet**
1. Open **MyRouteTable**.  
2. Click **Subnets** → **Associate**.  
3. Select your **VNet** (`MyVNet`).  
4. Choose the **Subnet** (e.g., `WebSubnet`).  
5. Click **OK**.  

---

## **Step 5: Configure Application Security Group (ASG)**
### **5.1 Create ASG**
1. Search for **Application Security Groups** in the Azure Portal.  
2. Click **+ Create**.  
3. Enter:  
   - **Name**: `WebServersASG`.  
   - **Subscription & Resource Group**: Same as VNet.  
   - **Region**: Same as VNet.  
4. Click **Review + Create**, then **Create**.  

### **5.2 Assign ASG to a VM**
1. Open a Virtual Machine (VM).  
2. Go to **Networking** → **Network Interface**.  
3. Under **IP Configurations**, click **Edit**.  
4. Select **WebServersASG**.  
5. Click **Save**.  

### **5.3 Define NSG Rules for ASG**
1. Open **MyNSG** → **Inbound Security Rules**.  
2. Click **+ Add Rule**.  
3. Enter:  
   - **Name**: `Allow-WebServers`.  
   - **Priority**: `200`.  
   - **Source**: `Application Security Group`.  
   - **Source ASG**: `WebServersASG`.  
   - **Destination**: `Any`.  
   - **Protocol**: `TCP`.  
   - **Port**: `80`.  
   - **Action**: `Allow`.  
4. Click **Add**.  

---

## **Step 6: Verify Configuration**
1. **Test Connectivity**  
   - Deploy a Virtual Machine (VM) in `WebSubnet` and `DBSubnet`.  
   - SSH or RDP into the VM.  
   - Use `ping` and `traceroute` to verify connectivity.  

2. **Check Network Security**  
   - Go to **NSG > Effective Security Rules** to confirm applied rules.  
   - Review logs in **Azure Monitor**.  

3. **Check Routing**  
   - Go to **Route Table > Effective Routes**.  
   - Ensure custom routes are applied correctly.  

---

## **Conclusion**
🎯 **You have successfully configured Azure Virtual Networking with:**
✅ **VNet & Subnets**  
✅ **NSGs & Custom Rules**  
✅ **Route Tables & Custom Routes**  
✅ **Application Security Groups (ASGs)**  

🚀 **Next Steps:**  
🔹 Deploy a Web App inside `WebSubnet`  
🔹 Configure **VPN Gateway** for hybrid networking  
🔹 Implement **Azure Firewall** for advanced security  

