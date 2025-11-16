# Overview
This project implements **Microsoft Entra Internet Access** to protect outbound internet traffic using:
* secure web gateway (SWG)
* URL filtering
* traffic forwarding
* Conditional Access
* identity-based policies
The result is a cloud-delivered, Zero-Trust-aligned replacement for traditional on-prem proxies.

---

# **Prerequisites**

* Microsoft Entra ID tenant
* Global Secure Access enabled
* At least one **licensed user** (Entra ID P1/P2)
* Test device (Windows 10/11)
* Microsoft Entra Admin Center access

---

# **Implementation Steps**

---

## **Step 1 — Enable Global Secure Access**

**Path:**
**Entra Admin Center → Global Secure Access (Preview)**

Enable the following:

* ✓ **Internet Access**
* ✓ **Unified Access Portal**
* ✓ **Global Secure Access Client download**

---

## **Step 2 — Download & Install the GSA Client**

1. Go to **Global Secure Access → Devices → Client software**
2. Download the Windows x64 installer
3. Install on the test computer
4. Sign in with Entra ID credentials
 see **“Traffic Forwarding: Enabled”** in the client UI.

---

##  **Step 3 — Create a Traffic Forwarding Profile**

**Path:**
**Global Secure Access → Internet Access → Traffic forwarding**

Create profile:
* Name: **Internet-Traffic-Forwarding**
* Type: **Internet Access**
* Assign:
  * security group: **GSA-InternetAccess-Users**

---

## **Step 4 — Configure Web Filtering Policies**
This is where you control the user’s internet activity.

**Path:**
**Global Secure Access → Internet Access → Web Content Filtering**

Create policy:

| Setting            | Value                               |
| ------------------ | ----------------------------------- |
| Name               | Corporate-Web-Policy                |
| Categories Blocked | Gambling, Adult, Malware, High-Risk |
| Allowed List       | *.microsoft.com                     |
| Block List         | facebook.com, tiktok.com            |

Enable:

* ✓ SSL inspection (optional)
* ✓ Logging to Unified Access Logs

---

## **Step 5 — Conditional Access Policy for Internet Access**

**Path:**
**Entra → Protection → Conditional Access**

Create CA policy:
**Assignments**
* Users: `GSA-InternetAccess-Users`
* Target resource: **Microsoft Entra Internet Access**
* Conditions:

  * Device = *Require compliant device*
  * Sign-in risk = *Medium and above* blocked

**Grant Controls:**

* Require MFA
* Require compliant device

---

## **Step 6 — Test Internet Access**

On the test machine:

1. Try browsing **blocked categories** → Should be denied
2. Try browsing **allowed domains** → Should work
3. Disconnect GSA Client → Access should be blocked (based on CA policy)

Document outcomes.

---

## **Step 7 — Monitor Traffic in Unified Access Logs**

**Path:**
**Global Secure Access → Monitoring → Unified Access Logs**

Check for:
* Allowed outbound traffic
* Blocked categories
* Policy hits
* Device identity
* User identity

---

# **Architecture Diagram (Add to GitHub)**

Include a diagram like this:

```
User Device → GSA Client → Traffic Forwarding → Entra Internet Access  
→ Web Filtering Policy → Internet  
→ Logs → Sentinel / Unified Access Logs
```

I can generate a **Mermaid diagram** for you if you want.

---

# **Results**

* Enforced secure web browsing for corporate users
* Blocked access to unauthorized and risky sites
* Centralized logging with full visibility
* Zero Trust access enforced using CA policies
* No VPN or on-prem proxy required
