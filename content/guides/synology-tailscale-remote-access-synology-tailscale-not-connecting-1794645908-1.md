---
title: "Fix Synology Tailscale Not Connecting: Connected but NAS Share Unreachable"
description: "Troubleshoot Synology Tailscale not connecting when Tailscale shows connected but SMB or DSM is unreachable. Step-by-step checks and verification."
seo_title: "Synology Tailscale Not Connecting: Fix Connected but NAS Share Unreachable"
seo_description: "Troubleshoot Synology Tailscale not connecting when Tailscale shows connected but SMB or DSM is unreachable. Step-by-step checks and verification."
keywords:
  - "synology tailscale not connecting"
date: 2026-11-14T19:47:13+08:00
publishDate: 2026-11-14T19:47:13+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected via Tailscale, the status says connected, but you can't reach your Synology NAS share or DSM—this guide walks you through a controlled diagnosis to restore access.

## Verify Tailscale IP and Ping
First, confirm the NAS's Tailscale IP address in the Tailscale admin console or on the NAS. From your remote device, ping that IP. If ping fails, the Tailscale tunnel isn't passing traffic—check that both devices are on the same tailnet and that the NAS's Tailscale service is running. If ping succeeds, proceed.

## Check SMB and DSM Services
On the NAS, open Control Panel > File Services and ensure SMB is enabled. For DSM, confirm HTTP/HTTPS services are running. Also verify the firewall: Control Panel > Security > Firewall. Create a rule allowing the Tailscale subnet (100.64.0.0/10) or the specific Tailscale interface. If the firewall blocks the Tailscale IP, connections will silently fail.

## Test Port Reachability
From the remote device, use telnet or a port checker to test TCP 445 (SMB) and 5000/5001 (DSM) on the NAS's Tailscale IP. A successful connection means the service is listening; a timeout points to firewall or service issues. This is a common cause of **synology tailscale not connecting** symptoms even when Tailscale itself reports connected.

## Verification Test
After adjustments, disconnect and reconnect Tailscale on the remote device, then try accessing \\<tailscale-ip>\share or https://<tailscale-ip>:5001. If it loads, the issue is resolved.

## Rollback Note
If changes break local access, revert firewall rules or disable the new rule temporarily. Keep a backup of firewall settings before editing.

If the share remains unreachable after these steps,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) for a guided review.
