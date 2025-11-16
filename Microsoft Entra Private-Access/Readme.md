# **Microsoft Entra Private Access**

# **Overview**

This project implements **Microsoft Entra Private Access** to securely publish **on-premises or private-network applications** (HTTP, RDP, SSH, SQL, etc.) using Zero Trust access controls.
It replaces traditional VPN by providing:
* Identity-based access to private apps
* Network segmentation
* Application discovery
* Conditional Access enforcement
* Traffic forwarding via the GSA Client

---

# **Prerequisites**
* Microsoft Entra tenant
* Global Secure Access enabled
* At least one on-prem/test application with an IP/hostname
* GSA Client installed on test device
* Test VM/app reachable from your home lab or simulation
* Security groups for assignment

---

# **Implementation Steps**

---

## **Step 1 — Enable Private Access**

**Path:**
**Entra Admin Center → Global Secure Access → Private Access**

Enable:

* ✓ Private Access
* ✓ Application Proxy integration
* ✓ Unified access logs

---

## **Step 2 — Create Security Group for Allowed Users**

Create group:

* **GSA-PrivateAccess-Users**

assign access to this group later.

---

## **Step 3 — Create an Enterprise Application**

**Path:**
**Entra Admin Center → Global Secure Access → Applications → + Add**

Choose:

* **Private access application**

Provide details:

| Field           | Value                     |
| --------------- | ------------------------- |
| Name            | Finance-App-01            |
| App Type        | Private App               |
| Internal URL/IP | `10.0.1.15`               |
| Protocol        | HTTP/HTTPS/RDP/SSH/Custom |
| Port            | e.g., 443 / 3389 / 22     |

---

## **Step 4 — Create Application Segments**

Application Segments define how traffic reaches your app.

**Path:**
**Global Secure Access → Private Access → Application Segments**

Create segment:

| Setting     | Value                           |
| ----------- | ------------------------------- |
| Name        | Finance-Segment                 |
| App Type    | TCP/UDP                         |
| Destination | `10.0.1.0/24` or host IP        |
| Port        | 443 (or the port your app uses) |

---

## **Step 5 — Assign Users / Groups to the App**

**Path:**
**Enterprise Applications → Finance-App-01 → Users & Groups**

Assign:

* **GSA-PrivateAccess-Users**

---

## **Step 6 — Configure a Conditional Access Policy**

Private Access can be protected using CA.

**Path:**
**Entra → Protection → Conditional Access**

Policy details:

**Assignments**

* Users: GSA-PrivateAccess-Users
* Cloud App: **Microsoft Entra Private Access**

**Conditions:**

* Device compliance required
* Block access if:

  * Sign-in risk = High
  * Location = Untrusted

**Grant Controls:**

* Require MFA
* Require compliant device

---

## **Step 7 — Install Profile on Device**

On your test workstation, the **GSA Client** pulls the assigned Private Access profiles.

Confirm:

* Open GSA Client
* Check **Private Access: Enabled**
* Check **Segments: Finance-Segment Loaded**

---

## **Step 8 — Test Private Access**

From the same client PC:

Try reaching the app or server:

Examples:

* Browser → `https://10.0.1.15`
* RDP → `10.0.1.100:3389`
* SSH → `10.0.1.200`

Expected behavior:

* Access works **only when GSA Client is connected**
* Access is denied when:

  * GSA client is disabled
  * Device is not compliant
  * CA blocks based on risk

---

## **Step 9 — Monitor Unified Access Logs**

**Path:**
**Global Secure Access → Monitoring → Unified Access Logs**

Look for:

* Allowed connections
* Denied connections
* Policy hits
* Device identity
* User identity
* Source/destination traffic

---

# **Architecture Diagram (Add to GitHub)**

```
User Device → GSA Client  
→ Traffic Forwarding  
→ Entra Private Access  
→ App Segments  
→ Internal Application (10.0.x.x)  
→ Logs → Unified Access Logs / Sentinel
```

---

# **Results**

* Eliminated VPN dependency
* Improved remote access security
* Restricted private apps to compliant devices only
* Full visibility into user & device behavior
* Strong Zero Trust enforcement

---
