# Azure-Compute-and-Networking



<p align="center">
  <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT_-QE9dNoPsL7EFaGjJ7CmWQmv31gGQXJxKw&s" alt="Azure Logo" width="200"/>
</p>

---

## 🔹 Part 1: Environment Setup  

In this section, we’ll set up the environment in Azure that will be used throughout the lab.

### 🛠️ Steps  

1. **Create a Resource Group**  
   - Open the Azure Portal.  
   - Create a new **Resource Group** (choose any name you like).  
   - Ensure all resources will be deployed to the **same region**.  
   - 📸 *[Screenshot Placeholder]*  

---

2. **Create Windows VM**  
   - Create a new **Windows 11 VM**.  
   - Name it: `windows-vm`.  
   - VM Size: Pick one with at least **2 vCPUs** and **8 GiB of memory**.  
   - Allow Azure to **create a new Virtual Network (VNet)** and **Subnet**.  
   - Place it in the Resource Group created earlier.  
   - 📸 *[Screenshot Placeholder]*  

---

3. **Create Linux VM**  
   - Create a new **Linux (Ubuntu) VM**.  
   - Name it: `linux-vm`.  
   - Use the **same Resource Group** and **same VNet** as `windows-vm`.  
   - Match similar sizing requirements (≥ 2 vCPUs, 8 GiB).  
   - 📸 *[Screenshot Placeholder]*  

---

✅ At this point, you should have two VMs (`windows-vm` and `linux-vm`) inside the same Resource Group and VNet. These will be used for communication and traffic observation in the next sections.
