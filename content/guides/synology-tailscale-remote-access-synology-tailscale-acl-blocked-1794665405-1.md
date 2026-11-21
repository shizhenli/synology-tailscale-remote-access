---
title: "Synology Tailscale ACL Blocked: Why the Firewall Drops Remote Connections"
description: "Tailscale shows connected but the Synology firewall drops remote access? Diagnose ACL and firewall rules to restore your remote connection."
seo_title: "Synology Tailscale ACL Blocked: Firewall Drops Remote Connection"
seo_description: "Tailscale shows connected but the Synology firewall drops remote access? Diagnose ACL and firewall rules to restore your remote connection."
keywords:
  - "synology tailscale acl blocked"
date: 2026-11-21T16:11:24+08:00
publishDate: 2026-11-21T16:11:24+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're connected to Tailscale, but the Synology firewall is dropping your remote connection, leaving you stuck at the firewall blocks tailscale traffic step. Let's diagnose this without guesswork.

## Check Tailscale ACLs First

Open your Tailscale admin console and go to the Access Controls tab. Look for any rule that might block traffic to your NAS. The default policy is allow-all, but if custom rules exist, verify that your user or device is permitted to reach the NAS on the required ports (typically 5000/5001 for DSM, 445 for SMB). If a deny rule appears before an allow rule, traffic is blocked. Result interpretation: if you see a deny rule matching your source, that's your culprit.

## Inspect Synology Firewall Rules

In DSM, open Control Panel > Security > Firewall. Check the active profile. Ensure there is an allow rule for the Tailscale interface (usually tailscale0) or for the Tailscale IP range (100.64.0.0/10). If the firewall is set to deny all and no allow rule exists for Tailscale, connections will drop. Result interpretation: a missing allow rule means the firewall is silently dropping packets.

## Verify with a Test

From your remote device, run `ping <NAS-Tailscale-IP>` then `curl -k https://<NAS-Tailscale-IP>:5001`. If ping succeeds but curl times out, the firewall is blocking the port. If both fail, check ACLs again. This isolates whether the block is at the ACL or firewall layer.

## Rollback and Recovery

Before changing firewall rules, export your current configuration. If a change breaks local access, you can restore the previous profile via DSM's firewall settings or by temporarily disabling the firewall through SSH. Always keep a local admin session open when editing rules.

If the **synology tailscale acl blocked** issue persists after these checks,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) for a guided review.
