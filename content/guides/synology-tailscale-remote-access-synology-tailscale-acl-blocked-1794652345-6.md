---
title: "Synology Tailscale ACL Blocked: Firewall Drops Remote Connection"
description: "Tailscale is up but Synology firewall drops remote connection. Follow these checks to diagnose and restore access when synology tailscale acl blocked."
seo_title: "Synology Tailscale ACL Blocked: Fix Firewall Drops"
seo_description: "Tailscale is up but Synology firewall drops remote connection. Follow these checks to diagnose and restore access when synology tailscale acl blocked."
keywords:
  - "synology tailscale acl blocked"
date: 2026-11-15T19:05:58+08:00
publishDate: 2026-11-15T19:05:58+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're connected to Tailscale, but the Synology firewall is silently dropping your remote connection, leaving you stuck at the firewall blocks Tailscale traffic step.

## Check the firewall rules order

Open Control Panel > Security > Firewall. Select the profile that applies to your Tailscale interface (usually the default or a custom profile). Look for a deny rule above any allow rule for the Tailscale subnet (100.64.0.0/10). If a deny-all rule sits higher than your allow rule, the firewall will drop Tailscale traffic before it reaches the NAS. Result interpretation: if the deny rule is above the allow rule, reorder by dragging the allow rule above it, then click Apply.

## Verify the Tailscale interface is trusted

In the same firewall profile, ensure the Tailscale interface (tailscale0) is selected under Interfaces. If it is not listed, add it. A missing interface means the firewall applies default rules that may block traffic. After adding, save and test.

## Confirm the ACL isn't blocking at the Tailscale level

Log in to the Tailscale admin console and check the ACL policy. Ensure your user or device has access to the NAS's Tailscale IP on the required ports (e.g., 5000/5001 for DSM). If the ACL denies, you'll see connection attempts time out even though the firewall is open. Adjust the ACL to allow the traffic, then save.

## Verification test

From your remote device, run `ping <NAS-Tailscale-IP>` and then `curl -k https://<NAS-Tailscale-IP>:5001`. If ping succeeds but curl fails, the firewall is still blocking the port. If both fail, check the ACL again. A successful curl returns the DSM login page HTML.

## Rollback note

If reordering rules causes loss of LAN access, revert to the previous firewall profile by clicking Restore default settings, then reapply only the necessary allow rules. Always export the firewall configuration before changes.

If the **synology tailscale acl blocked** issue persists after these steps,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) for a guided diagnosis.
