
---

#  Wazuh SIEM Setup in My Homelab

In this part of the project, I set up **Wazuh SIEM** on an Ubuntu server to monitor the activities of my Windows 10 client. The idea was to simulate how organizations use a **Security Information and Event Management (SIEM)** tool to collect logs, detect threats, and analyze incidents from multiple machines.

---

##  Installing Wazuh on Ubuntu

I started with a clean Ubuntu Server VM and installed Wazuh using the all-in-one deployment method. I refer the wazuh installation document, it's very helpful and easy for installation process

**Steps I followed:**

1. Updated the system:

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Downloaded and ran the Wazuh installation script:

   ```bash
   curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
   sudo bash wazuh-install.sh -a
   ```
3. After installation, Wazuh services (Manager, API,Indexer, Dashboard) were running.
4. Accessed the Wazuh Dashboard in my browser using the Ubuntu server’s IP:

   ```
   https://<ubuntu-ip>
   ```
5. Logged in with the default credentials (then changed to a strong password).

---

##  Preparing to Add Agents

A SIEM is only useful when it collects logs. My plan was to add my **Windows 10 client** as an agent so that all security events and logs could be monitored centrally in Wazuh.

---

## Installing the Agent on Windows 10

**Steps I followed:**

1. Downloaded the Windows Wazuh agent from the wazuh dashboard and select a new deploy agent
2. During installation, I entered:

   * **Wazuh server IP** → (Ubuntu server’s IP).
   * **Agent name** → same as my Windows hostname.
3. After installation, the agent appeared on my Wazuh Dashboard.
4. Restarted the Wazuh agent service:

   ```powershell
   Restart-Service WazuhSvc
   ```

---

##  Verifying the Agent Connection

* On the Wazuh Dashboard, my Windows 10 machine appeared as “connected”.
* I checked the logs, and events like **user logins, system activity, and security alerts** were being sent to Wazuh.
* I tested by logging in/out of Windows and could immediately see events in the SIEM dashboard.
* I added rules that is its show alert when user created or deteted the files and also  the newly creaetd file will scanned by virustotal 

---

## Results

* The **Wazuh server** was running successfully on Ubuntu.
* The **Windows 10 client** was added as an agent and sending logs.
* I could see security events in real-time, such as:

  * User logins
  * Policy updates
  * System warnings

---

##  Key Takeaway

This setup gave me hands-on experience with how enterprises use **SIEM solutions** like Wazuh to:

* Collect logs from endpoints,
* Detect unusual activity,
* And provide visibility into the security posture of the network.

By combining this with my **proxy server** and **Active Directory**, I created a small-scale **SOC-style environment** where network control and monitoring are integrated.

---

