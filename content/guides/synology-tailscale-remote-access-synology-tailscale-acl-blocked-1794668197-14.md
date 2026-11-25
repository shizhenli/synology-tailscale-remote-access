---
title: "Synology Tailscale ACL Blocked: When the Firewall Drops Remote Connections"
description: "Troubleshoot Synology Tailscale ACL blocked: firewall drops remote connection. Step-by-step checks to restore access without breaking security."
seo_title: "Fix Synology Tailscale ACL Blocked: Firewall Drops Remote Connection"
seo_description: "Troubleshoot Synology Tailscale ACL blocked: firewall drops remote connection. Step-by-step checks to restore access without breaking security."
keywords:
  - "synology tailscale acl blocked"
date: 2026-11-25T08:08:33+08:00
publishDate: 2026-11-25T08:08:33+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely accessing your Synology NAS via Tailscale, but the Synology firewall is dropping the connection even though Tailscale shows as connected.

## Check Tailscale Interface and Firewall Rules
First, confirm Tailscale is running on the NAS. Open Package Center and ensure Tailscale is active. Then go to Control Panel > Security > Firewall. Check if there's a rule blocking the Tailscale interface (usually tailscale0). The firewall may have a default deny policy that doesn't allow traffic from the Tailscale subnet (100.64.0.0/10). Look for any rule that explicitly denies traffic from that range or from the tailscale0 interface. If you find one, note its position—rules are processed top-down.

## Verify ACL and Firewall Order
If you use Synology's built-in firewall, ensure a rule allowing Tailscale traffic is placed above any deny rules. A common mistake is having a deny-all rule before the Tailscale allow rule. Also, check Tailscale ACLs in your admin console: ensure the NAS's tags or user have access to the required ports (e.g., 5000/5001 for DSM). If the ACL denies, Tailscale won't route traffic, but the Synology firewall might still drop packets if misconfigured. This is often the root of a **synology tailscale acl blocked** scenario.

## Verification Test
From a remote device on Tailscale, run `ping <NAS-Tailscale-IP>`. If it fails, the firewall is blocking ICMP. Then try `telnet <NAS-Tailscale-IP> 5000`. If connection refused, the service isn't listening; if timeout, firewall drops. Temporarily disable the Synology firewall (Control Panel > Security > Firewall > Disable) and retest. If it works, the firewall is the culprit.

## Rollback and Recovery
If disabling the firewall restores access, re-enable it and add an allow rule for the Tailscale interface and subnet. Test again. If issues persist, revert to the previous firewall configuration and consult the guide. For personalized help,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
