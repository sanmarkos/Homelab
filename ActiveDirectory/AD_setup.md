
---

# 🏢 Active Directory Setup in My Homelab

In this part of the lab, I built a small **Active Directory (AD)** environment to simulate how companies manage users, computers, and policies. The goal was to centralize user management, connect machines to a domain, and enforce rules like proxy settings.

---

##  Domain Controller Setup

I started with my Windows Server and installed the **Active Directory Domain Services (AD DS)** role. Then I promoted the server to a **Domain Controller** and created my own domain:

```
markos.com
```

To make sure everything worked smoothly, I also set up **DNS** on the same server so the domain and machines could communicate properly.

---

## Creating Users

I wanted to make the environment realistic, so I added some test users.

**Steps I followed:**

1. Opened **Active Directory Users and Computers (ADUC)**.
2. Created a new **Organizational Unit (OU)** called `Quality Dept`.
3. Inside the OU, I added a user accounts. Example:

   * Username → `QA1`
   * Password → set a default and checked *“User must change password at next logon”*.

This way, every new user gets a fresh password when they log in the first time — exactly like in a real company.

---

## Joining a Computer to the Domain

Next, I joined my Windows 10 machine to the domain so that it could use AD policies.

**Steps I followed:**

1. On Windows 10, I changed the **DNS server** to point to my Domain Controller’s IP (192.168.10.x).
2. Went to → *System Properties → Computer Name → Change settings*.
3. Selected **Domain** and entered `markos.com`.
4. Entered my AD credentials to authorize the join.
5. Restarted the computer.

After restarting, I could log in with the domain account (e.g., `MARKOS\QA1`) instead of just the local account.

---

##  Applying Group Policy (GPO)

Once the domain and users were ready, I wanted to enforce proxy settings automatically through **Group Policy**.

**Steps I followed:**

1. Opened **Group Policy Management Console (GPMC)** on the server.
2. Created a new GPO called `ProxyPolicy`.
3. Linked the GPO to the `Quality Dept` OU.
4. Edited the GPO →

   * Path: *User Configuration → Preferences → Control Panel Settings → Internet Settings*.
   * Enabled proxy: `192.168.10.20` (Kali Squid server) on port `3128`.
5. On Windows 10, ran `gpupdate /force` to apply it immediately.

Now, whenever a domain user logs in, the proxy is already set.

---

##  Results

* My **Windows 10 client successfully joined the domain** (`markos.com`).
* **Domain users** could log in from any domain-joined machine.
* The **proxy settings were applied automatically**.
* Testing showed:

  *  Google worked.
  *  Facebook was blocked by my Squid Proxy rules.

---

## Key Takeaway

This setup gave me hands-on experience with how **Active Directory** is used in enterprises to:

* Manage users and computers in one place,
* Enforce security and network rules through GPO,
* Integrate with other services like proxies and monitoring tools.

It helped me better understand how IT teams control access and maintain consistency across all employee machines.
