# **Creating a Virtual Machine (VM) in Azure using Azure CLI**  

## **Introduction**  
Azure CLI (Command-Line Interface) is a powerful tool for managing Azure resources directly from a terminal. This guide will walk you through **creating, managing, and deleting an Azure Virtual Machine (VM)** using Azure CLI.  

---

## **Prerequisites**  
Before you begin, ensure that:  
1. You have an **active Azure subscription**.  
2. You have **Azure CLI installed** on your system. You can install it from: [Azure CLI Installation Guide](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)  
3. You have **logged into Azure CLI** using:  
   ```bash
   az login
   ```
   This command will open a browser window for authentication. If using a VM or remote server, use:  
   ```bash
   az login --use-device-code
   ```
4. Your Azure CLI version is up to date:  
   ```bash
   az upgrade
   ```

---

## **Step 1: Create a Resource Group**  
A **Resource Group** is a container that holds related Azure resources. To create a resource group, run:  

```bash
az group create --name learn-azure-cli --location eastus
```

### **Explanation:**  
- `az group create` → Creates a new resource group.  
- `--name learn-azure-cli` → Names the resource group as `learn-azure-cli`.  
- `--location eastus` → Specifies the Azure region (`eastus`). You can find available regions using:  
  ```bash
  az account list-locations --output table
  ```

---

## **Step 2: Set the Resource Group as Default (Optional)**  
To avoid specifying the resource group in every command, you can set it as default:  

```bash
az config set defaults.group=learn-azure-cli
```

Now, any command that requires a resource group will automatically use `learn-azure-cli`.

---

## **Step 3: Create a Virtual Machine (VM) with a Virtual Network (VNet)**  

```bash
az vm create \
  --resource-group learn-azure-cli \
  --name MyAzureVM \ 
  --image Ubuntu2204 \
  --vnet-name MyVnet \  
  --subnet MySubnet \    
  --generate-ssh-keys \
  --output json \
  --verbose
```

### **Explanation:**  
- `az vm create` → Creates a new virtual machine.  
- `--resource-group learn-azure-cli` → Uses the `learn-azure-cli` resource group.  
- `--name MyAzureVM` → Assigns the VM name as `MyAzureVM`.  
- `--image Ubuntu2204` → Uses an **Ubuntu 22.04 LTS** image. You can use other images like:  
  - Windows Server: `Win2022Datacenter`  
  - Red Hat Linux: `RHEL8`  
  - CentOS: `CentOS8_3`  
  - View all available images using:  
    ```bash
    az vm image list --output table
    ```  
- `--vnet-name MyVnet` → Specifies the **Virtual Network (VNet)**.  
- `--subnet MySubnet` → Uses a **Subnet** within the VNet.  
  - If you haven't created a VNet and Subnet yet, you can do so with:  
    ```bash
    az network vnet create --name MyVnet --resource-group learn-azure-cli --subnet-name MySubnet
    ```
- `--generate-ssh-keys` → Generates SSH keys for authentication.  
- `--output json` → Displays results in **JSON format** (you can also use `table` or `yaml`).  
- `--verbose` → Provides **detailed** logs for debugging.  

**After running the command, Azure CLI will output details of the created VM, including its IP address.**  

---

## **Step 4: Connect to the Virtual Machine**  
Once the VM is created, you can connect to it via SSH:  

```bash
ssh azureuser@<Public-IP>
```

To get the **Public IP address** of the VM, run:  
```bash
az vm list-ip-addresses --name MyAzureVM --output table
```

---

## **Step 5: List Virtual Machines**  
To check all VMs in your subscription:  
```bash
az vm list --output table
```
To check details of a specific VM:  
```bash
az vm show --name MyAzureVM --resource-group learn-azure-cli --output table
```

---

## **Step 6: Start, Stop, and Restart the VM**  

To **start** the VM:  
```bash
az vm start --name MyAzureVM --resource-group learn-azure-cli
```

To **stop** the VM:  
```bash
az vm stop --name MyAzureVM --resource-group learn-azure-cli
```

To **restart** the VM:  
```bash
az vm restart --name MyAzureVM --resource-group learn-azure-cli
```

To **deallocate** (stop and release resources):  
```bash
az vm deallocate --name MyAzureVM --resource-group learn-azure-cli
```

---

## **Step 7: Delete the VM**  
If you no longer need the VM, delete it with:  
```bash
az vm delete --name MyAzureVM --resource-group learn-azure-cli --yes
```
The `--yes` flag **confirms** deletion without asking for manual input.

---

## **Step 8: Delete the Resource Group (Removes All Resources)**  
To clean up all resources (including the VM, VNet, and other services), delete the entire resource group:  

```bash
az group delete --name learn-azure-cli --yes --no-wait
```

### **Explanation:**  
- `az group delete` → Deletes the resource group and all its resources.  
- `--name learn-azure-cli` → Specifies the resource group to delete.  
- `--yes` → Confirms deletion without prompting.  
- `--no-wait` → Deletes resources **asynchronously** (does not wait for completion).  

⚠ **Warning:** This action **cannot be undone** and will delete **all** resources inside the resource group.

---

## **Summary of Commands**
| **Task** | **Command** |
|----------|------------|
| Log in to Azure CLI | `az login` |
| Create a resource group | `az group create --name learn-azure-cli --location eastus` |
| Set default resource group (optional) | `az config set defaults.group=learn-azure-cli` |
| Create a VM with a VNet | `az vm create --resource-group learn-azure-cli --name MyAzureVM --image Ubuntu2204 --vnet-name MyVnet --subnet MySubnet --generate-ssh-keys --output json --verbose` |
| Connect to the VM via SSH | `ssh azureuser@<Public-IP>` |
| List all VMs | `az vm list --output table` |
| Get VM details | `az vm show --name MyAzureVM --output table` |
| Start the VM | `az vm start --name MyAzureVM` |
| Stop the VM | `az vm stop --name MyAzureVM` |
| Restart the VM | `az vm restart --name MyAzureVM` |
| Deallocate the VM | `az vm deallocate --name MyAzureVM` |
| Delete the VM | `az vm delete --name MyAzureVM --yes` |
| Delete the resource group | `az group delete --name learn-azure-cli --yes --no-wait` |


Would you like to extend this guide with **auto-scaling, VM monitoring, or security best practices?** Let me know! 🚀
