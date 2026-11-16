---
title: "Synology Remote Access Cellular: Tailscale Shows Connected but SMB/DSM Unreachable"
description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable over cellular? Follow these practical checks to diagnose and restore access."
seo_title: "Synology Remote Access Cellular: Fix Tailscale Connected but NAS Share Unreachable"
seo_description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable over cellular? Follow these practical checks to diagnose and restore access."
keywords:
  - "synology remote access cellular"
date: 2026-11-16T08:08:14+08:00
publishDate: 2026-11-16T08:08:14+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're on cellular, Tailscale says connected, but SMB or DSM still won't open on your Synology NAS.

## Confirm the Tailscale IP and port
On the remote device, open Tailscale and note the NAS's Tailscale IP (100.x.x.x). In a browser, try http://100.x.x.x:5000 (DSM) and smb://100.x.x.x (SMB). If DSM loads but SMB fails, the issue is SMB-specific; if both fail, it's routing or firewall.

## Check the NAS firewall
In DSM, open Control Panel > Security > Firewall. Ensure the Tailscale interface (tailscale0) is allowed, or temporarily disable the firewall and retest. If access returns with the firewall off, add a rule permitting the Tailscale subnet 100.64.0.0/10 on ports 5000/5001 and 445.

## Verify SMB service and share permissions
In Control Panel > File Services > SMB, confirm SMB is enabled. Then check Shared Folder permissions for your user. If DSM works but SMB doesn't, this is the usual culprit.

## Test from another Tailscale device
From a second device on the same tailnet, ping the NAS Tailscale IP. A reply confirms the tunnel; no reply points to ACLs or the NAS Tailscale service. This is also a good check for **synology remote access cellular** reliability.

## Verification test
Reconnect cellular, open smb://100.x.x.x and DSM at :5000. Both should load. If only one does, revisit the matching section above.

## Rollback note
If a firewall change breaks local LAN access, re-enable the previous rule or disable the new one. Keep a local admin session open while testing.

Still stuck?

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
