# Azure Compute and Networking



<p align="center">
  <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT_-QE9dNoPsL7EFaGjJ7CmWQmv31gGQXJxKw&s" alt="Azure Logo" width="200"/>
</p>

---

## 🔹 Part 1: Environment Setup  

In this section, we’ll set up the environment in Azure that will be used throughout the lab.

### 🛠️ Steps  

1. **Create a Resource Group**  
   - Open the Azure Portal.  
   - Create a new **Resource Group** (you can name it something like `Networking_Lab`).  
   - Ensure all resources will be deployed to the **same region**.  



2. **Create Windows Virtual Machine**  
   - Create a new **Windows 11 VM**.  
   - Name it: `windows-vm`.  
   - VM Size: Pick one with at least **2 vCPUs** and **8 GiB of memory**.  
   - Allow Azure to **create a new Virtual Network (VNet)** and **Subnet**.  
   - Place it in the Resource Group created earlier.  
   <p align="center">
     <img src="https://i.imgur.com/1tx1qnx.png" alt="Windows VM Creation" width="500"/>
   </p>


3. **Create Linux VM**  
   - Create a new **Linux (Ubuntu) VM**.  
   - Name it: `linux-vm`.  
   - Use the **same Resource Group** and **same VNet** and **subnet** as `windows-vm`.  
   - Match similar sizing requirements (≥ 2 vCPUs, 8 GiB).  
   <p align="center">
     <img src="https://i.imgur.com/xszuhlc.png" alt="Linux VM Creation" width="600"/>
   </p>

---





## 🔹 Part 2: Packet Capture and ICMP Testing  

In this section, we’ll log into the VMs, install Wireshark, and analyze network traffic between them.  

### 🛠️ Steps  

1. **Log into Both VMs**  
   - Use **RDP** from the Azure Portal to connect to both `windows-vm` and `linux-vm`.  


2. **Install Wireshark on Windows VM**  
   - On the `windows-vm`, search for **Wireshark** in a browser and download the Windows installer.  
   - Complete the installation using default settings.  

   <p align="center">
     <img src="https://i.imgur.com/IiMcqAS.png" alt="Installing Wireshark" width="600"/>
   </p>


3. **Start a Packet Capture**  
   - Open Wireshark on the `windows-vm`.  
   - Select the network interface and click **Start Capturing**.  
   - At this stage, you’ll see lots of background traffic flowing.  

   > **ℹ️ What is an IP Packet?**  
   > An IP packet is the basic unit of data that travels across a network.  
   > It contains a **header** (with source and destination addresses) and a **payload** (the actual data being sent).  

   <p align="center">
     <img src="https://i.imgur.com/k6ZUnom.png" alt="Initial Packet Capture" width="600"/>
   </p>


4. **Filter for ICMP Traffic**  
   - In Wireshark, enter `icmp` in the filter bar.  
   - Notice that at first, no ICMP traffic is displayed.  



5. **Ping Linux VM from Windows VM**  
   - Go back to the Azure Portal.  
   - Retrieve the **private IP address** of the `linux-vm`.  

   <p align="center">
     <img src="https://i.imgur.com/jLlEQLq.png" alt="Retrieving Linux VM Private IP" width="600"/>
   </p>  

   - On the `windows-vm`, open **PowerShell** (or Command Prompt).  
   - Run the following command:  
     ```powershell
     ex) ping <172.16.0.5>
     ```  

   <p align="center">
     <img src="https://i.imgur.com/K1B0XNH.png" alt="Pinging Linux VM from Windows VM" width="600"/>
   </p>



6. **Observe Packets in Wireshark**  
   - Switch back to Wireshark.  
   - You should now see the ICMP Echo Request (ping) and Echo Reply packets between the two VMs.  

   <p align="center">
     <img src="https://i.imgur.com/cFW8ykV.png" alt="ICMP Traffic Observed in Wireshark" width="600"/>
   </p>

---



## 🔹 Part 3: Network Security Group (Firewall) Rules  

In this section, we’ll use Azure Network Security Groups (NSGs) to control traffic between the VMs. Specifically, we’ll block ICMP traffic and observe the results.  

### 🛠️ Steps  

1. **Initiate a Continuous Ping**  
   - On the `windows-vm`, open **PowerShell**.  
   - Start a continuous ping to the private IP of `linux-vm`:  
     ```powershell
     ex) ping 172.16.0.5 -t
     ```  

   <p align="center">
     <img src="https://i.imgur.com/OwYTNCx.png" alt="Continuous Ping to Linux VM" width="600"/>
   </p>  

   > 🔎 **Note:** To stop a continuous ping, press **Ctrl + C**. This will stop the process and display a summary of packets sent/received.  

---

2. **Create a Network Security Group (NSG) Rule**  
   - In the Azure Portal, open the **Network Security Group** associated with `linux-vm`.  
   - Add a new **Inbound Security Rule** with these settings:  
     - **Protocol:** ICMP  
     - **Action:** Deny  
     - **Priority:** e.g., 100 (lower than default allow rules)  

   <p align="center">
     <img src="https://i.imgur.com/nDZV6xC.png" alt="Deny ICMP Rule in NSG" width="600"/>
   </p>  

---

3. **Observe Behavior in PowerShell**  
   - On the `windows-vm`, What do you notice?
   - The ping results should now show **Request Timed Out**, since ICMP replies from `linux-vm` are being blocked.  

   <p align="center">
     <img src="https://i.imgur.com/9C9AfUL.png" alt="Ping Results After ICMP Block" width="600"/>
   </p>  

---

4. **Observe Behavior in Wireshark**  
   - In Wireshark, you’ll see ICMP **Echo Requests** continue to leave the `windows-vm`, but **no Echo Replies** are coming back.  
   - This shows the firewall rule is dropping inbound ICMP traffic.  

   <p align="center">
     <img src="https://i.imgur.com/detD05P.png" alt="ICMP Traffic Blocked in Wireshark" width="600"/>
   </p>  

---

### 🔑 Recap & Real-World Relevance  

In this section, we saw how creating an NSG rule in Azure blocked ICMP traffic between two VMs. As an IT specialist, this directly relates to real-world tasks such as:  

- **Troubleshooting connectivity issues** (knowing when traffic is being blocked by a firewall vs when the endpoint itself is down).  
- **Implementing security policies** by controlling what kinds of traffic are allowed or denied.  
- **Understanding packet behavior** and being able to confirm changes using tools like PowerShell and Wireshark.  

👉 In practice, IT professionals regularly configure firewalls/NSGs, monitor traffic, and verify the effects of those changes to ensure both **security** and **functionality** of systems.

---

## 🔹 Part 4: Observing SSH Traffic  

In this section, we’ll initiate an SSH connection from the `windows-vm` to the `linux-vm` and capture the traffic in Wireshark.  

### 🛠️ Steps  

1. **Retrieve Linux VM Private IP**  
   - Go to the Azure Portal and confirm the **private IP address** of `linux-vm` (in this example: `172.16.0.5`).  

   <p align="center">
     <img src="https://i.imgur.com/WDtv66H.png" alt="Linux VM Private IP" width="600"/>
   </p>  

---

2. **SSH into Linux VM from Windows VM**  
   - On the `windows-vm`, open **PowerShell**.  
   - Enter the following command:  
     ```powershell
     ssh labuser@<private-ip>
     ```  
   - When prompted with a security/host authenticity question, type **yes**.  
   - Enter the password you set for the Linux VM.  
   - You should now be logged into the Linux VM via SSH.  

   <p align="center">
     <img src="https://i.imgur.com/BufwuYs.png" alt="SSH into Linux VM" width="600"/>
   </p>  

---

3. **Filter SSH in Wireshark**  
   - On the `windows-vm`, go back to Wireshark.  
   - Apply a filter for:  
     ```
     ssh
     ```  
     or  
     ```
     tcp.port == 22
     ```  
   - You’ll see packets exchanged between the Windows and Linux VMs.  
   - The communication is encrypted, so you cannot read the actual contents.  

   <p align="center">
     <img src="https://i.imgur.com/PbGSPTA.png" alt="Observing SSH Traffic in Wireshark" width="600"/>
   </p>  

---

4. **Run Commands in the SSH Session**  
   - Once logged into the `linux-vm`, type commands such as:  
     ```bash
     hostname
     id
     uname -a
     ```  
   - In PowerShell, you’ll see the Linux system’s responses (hostname, user info, OS details).  

   <p align="center">
     <img src="https://i.imgur.com/VUZlbGs.png" alt="Testing Commands in SSH Session" width="600"/>
   </p>  

   - Back in Wireshark, you’ll notice:  
     - Packets are actively exchanged whenever you type.  
     - **Every keystroke and command generates SSH packets.**  
     - The **contents are encrypted** — Wireshark shows the packets, but not the actual commands or outputs.  

---

### 📘 Key Insight  

- SSH protects sensitive data by encrypting both the commands sent and the responses received.  
- Even though Wireshark shows packet activity, the payloads cannot be read.  
- This is different from ICMP:  
  - **ICMP:** You can see Echo Requests/Replies and their contents.  
  - **SSH:** You only see encrypted packets, not the actual command text.  
- For IT specialists, this highlights why SSH is the standard for remote access, replacing insecure options like Telnet (which would expose everything in plain text).  

---
