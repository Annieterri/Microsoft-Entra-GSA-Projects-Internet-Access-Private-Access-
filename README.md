# Microsoft-Entra-GSA-Projects-Internet-Access-Private-Access-
Hands-on implementation projects using Microsoft Entra Global Secure Access.

This repository documents my practical work with Microsoft Entra Internet Access (MIAA) and Microsoft Entra Private Access (MIPA), focusing on Zero Trust Network Access (ZTNA), secure web filtering, private resource access, conditional access segmentation, and traffic inspection.

# Projects Included
## 1. Microsoft Entra Internet Access (Secure Web Gateway)
A full implementation of Microsoft’s cloud-based secure web gateway to protect outbound internet traffic.
* Configured Global Secure Access Client on Windows devices
* Enabled secure web gateway policies
* Enforced allow/block lists, categories, and safe browsing
* Applied Conditional Access policies to enforce ZTNA for web browsing
* Configured traffic inspection and TLS inspection support

## 2. Microsoft Entra Private Access (ZTNA / VPN Replacement)
A full Zero Trust remote access setup for internal line-of-business apps.
* Created Enterprise Applications for internal resources (DNS, IP, FQDN-based)
* Configured Application Segments with ports & protocols
* Enforced device compliance and MFA before accessing private resources
* Applied Conditional Access policies to enforce per-app segmentation
* Configured Global Secure Access Client with Private Access profile

## 3. Conditional Access for GSA (Zero Trust Architecture)
Policies designed to enforce the right access for the right user, device, and application.
### Implemented
* CA policy for Private Access (only compliant devices)
* CA policy for Internet Access (block unsanctioned apps)
* CA policy for High-Risk sign-ins
* CA policy to enforce GSA Client required before accessing internal apps
* Tested session controls, user risk, and sign-in risk
* Applied per-user, per-group segmentation

# Tech Stack
Microsoft Entra Global Secure Access
Microsoft Entra Internet Access
Microsoft Entra Private Access
Conditional Access
Microsoft Intune
Zero Trust Framework
Microsoft Sentinel
Application Segmentation
Web Filtering Policies
KQL
