# Homelab Project - Firewall, Proxy, AD & SIEM Setup
## Overview      
This homelab is designed to **simulate a small enterprise IT environment** using virtual machines and open-source tools
It brings together core technologies like **Active Directory**, **SIEM monitoring**, and a **Proxy/Firewall**, so I can learn and practice in a hands-on way.  

---

## Project Structure
```
homelab/
│── ActiveDirectory/        # Notes & steps for AD setup
│ ├── AD_setup.md
│── WazuhSIEM/              # Notes & steps for Wazuh setup
│ ├── wazuh_setup.md
│ └── SIEM_report  
│── ProxyFirewall/          # Notes for Squid Proxy & firewall
│ ├── squid.config          # This file have proxy rules
│ └── access.log
│── Diagram/                # Draw.io architecture diagram
│── Screenshots/            # Screenshot for each opertion 
│── README.md               # Documentation

```
  
---
#  What this project covers

### 1.  Active Directory
- Installed and configured Windows Server as a Domain Controller.
- Created domain users and computers.
- Joined a Windows 10 client to the domain.
- Applied Group Policy to enforce proxy settings.

### 2. Firewall & Proxy (Kali Linux)
- Configured Squid proxy to control internet access.
- Created ACLs to allow/block specific websites.
- Configured browser settings to use proxy.
- Verified logging of user activity.

### 3.  Wazuh SIEM (Ubuntu)
- Installed Wazuh server to monitor endpoints.
- Connected Windows 10 and Ubuntu as Wazuh agents.
- Verified logs in the Wazuh dashboard.


##  Components  

- **Windows Server 2019** → Active Directory Domain Controller (DNS, GPO)  
- **Windows 10 Client** → Joined to AD, policies enforced  
- **Ubuntu** → Wazuh SIEM & Dashboard 
- **Kali Linux** → Squid Proxy + Firewall rules
- **Windows 10** → connected to Wazuh agent to monitor the machine 
---

## Workflow  

Here’s how everything connects:  

- Users log in to **Windows 10** using **AD accounts**.  
- **Group Policies (GPOs)** from the Domain Controller enforce security settings.  
- **All traffic** from Windows 10 → routed via **Kali Linux Proxy (Squid)**.  
- Squid decides what websites are allowed or blocked, and logs activity.  
- System logs from Windows clients → sent to **Wazuh SIEM** for monitoring.  
- **Wazuh Dashboard** helps visualize alerts and potential threats.  

---

# Homelab Workflow
```
                  ┌───────────────┐
                  │   Internet    │
                  └───────▲───────┘
                          │
                          │
                ┌─────────────────────┐
                │  Kali Linux Proxy   │
                │ (Firewall + Squid)  │
                └───┬─────┬─────┬─────┘
                    │     │     │
        ┌───────────┘     │     └───────────┐
        │                 │                 │
┌───────▼────────┐  ┌─────▼─────┐   ┌───────▼─────────┐
│ Windows Server │  │ Wazuh SIEM│   │  Ubuntu Client  │
│ (Active Dir +  │  │  (Ubuntu) │   │ (Monitored by   │
│ DNS + GPO)     │  └───────────┘   │    Wazuh)       │
└───────┬────────┘                  └────────┬────────┘
        │                                    │
        │ Policies                           │ Traffic + Logs
        │                                    │
┌───────▼────────┐                   ┌───────▼─────────┐
│ Windows 10 PC  │ ----------------▶│   Wazuh SIEM     │
│ (Domain-joined │   Logs + Alerts   │  Dashboard      │
│   workstation) │                   └─────────────────┘
└────────────────┘
```

---

## Future Improvements

* Add a **VPN gateway** for secure external access.
* Replace Squid with **pfSense** for enterprise-grade proxy/firewall.
* Simulate **cyberattacks** and test SIEM alerting.
* Automate VM deployment with Ansible/Terraform.

---

##  Why This Project?

I wanted to learn like in a real company, where:

* One server manages users (AD).
* One system watches security logs (Wazuh).
* One server filters traffic (Proxy).
* All machines are connected in a workflow.

This way, I understand not just the tools, but also how they interact in an enterprise setup.

---
## Tested On
- Windows Server 2019 
- Windows 10 Pro 
- Kali Linux 2024.2 
- Ubuntu Server 22.04

## Author 
**Sutharsan**

sutharsansenthil46@gmail.com
