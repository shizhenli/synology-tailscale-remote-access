---
title: "Synology Remote Access Cellular: Tailscale Connected but SMB/DSM Unreachable"
description: "Troubleshoot Tailscale showing connected but your Synology NAS share or DSM unreachable over cellular. Step-by-step checks for SMB, firewall, and subnet routing."
seo_title: "Synology Remote Access Cellular: Fix Tailscale Connected but NAS Share Unreachable"
seo_description: "Troubleshoot Tailscale showing connected but your Synology NAS share or DSM unreachable over cellular. Step-by-step checks for SMB, firewall, and subnet routing."
keywords:
  - "synology remote access cellular"
date: 2026-11-26T08:06:55+08:00
publishDate: 2026-11-26T08:06:55+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You are on cellular, Tailscale says connected, but your Synology NAS share or DSM page will not open, and you want a controlled way to find the break.

## Confirm what "connected" actually means

On the remote device, open Tailscale and check the NAS entry. A green dot only means the tunnel is up; it does not prove SMB (445) or DSM (5000/5001) is reachable. Note the NAS Tailscale IP (100.x.x.x). If the entry is relayed (DERP), latency will be high but still workable.

## Test the path before touching settings

From the remote device, ping the NAS Tailscale IP. Success means routing is fine and the problem is service-level. Then try DSM in a browser at http://100.x.x.x:5000. If DSM loads but SMB fails, the issue is SMB-specific; if neither loads, suspect the NAS firewall or Tailscale ACLs.

## Check the NAS firewall and SMB service

In DSM, open Control Panel > Security > Firewall. Ensure the Tailscale interface (tailscale0) is allowed, or temporarily disable the firewall to test. Then verify SMB is enabled under File Services and that the shared folder permissions include your account. On cellular, some carriers block port 445; test SMB over Tailscale only, not the public IP.

## Verify with a second client

Connect a laptop to the same Tailscale tailnet and open the NAS share. If it works there but not on cellular, the mobile device's Tailscale app or carrier is the variable; reinstall the app or toggle airplane mode.

## Rollback note

If a firewall or ACL change breaks access, revert the last rule and re-enable the previous state before retesting. Keep a note of the original rule order.

For a guided diagnosis of your **synology remote access cellular** setup,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
