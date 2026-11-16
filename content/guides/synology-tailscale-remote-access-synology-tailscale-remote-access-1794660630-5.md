---
title: "Synology Tailscale Connected but NAS Share Unreachable: A Step-by-Step Fix"
description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow these checks to diagnose and restore SMB/DSM access over Tailscale."
seo_title: "Synology Tailscale Connected but NAS Share Unreachable — Fix"
seo_description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow these checks to diagnose and restore SMB/DSM access over Tailscale."
keywords:
  - "synology tailscale remote access"
date: 2026-11-16T10:08:16+08:00
publishDate: 2026-11-16T10:08:16+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected through Tailscale, the status says connected, but SMB or DSM on your Synology NAS won't open — this guide walks you through a controlled diagnosis to restore that share.

## Confirm the Tailscale IP and Ping

On the remote device, open Tailscale and note the NAS's 100.x.y.z address. Ping it: `ping 100.x.y.z`. If replies come back, the tunnel is healthy and the problem is above the network layer. If it times out, stop here — the issue is Tailscale routing, not SMB.

## Test the Ports Directly

From the remote device, test SMB (445) and DSM (5000/5001). On Windows: `Test-NetConnection 100.x.y.z -Port 445`. On macOS/Linux: `nc -vz 100.x.y.z 445`. A success means the NAS is listening; a timeout points to the Synology firewall or the service being off. Check Control Panel > Security > Firewall for a rule allowing the Tailscale subnet, and confirm SMB is enabled under File Services.

## Verify Name Resolution vs IP

Try connecting by IP first: `\\100.x.y.z\share` or `https://100.x.y.z:5001`. If the IP works but the hostname fails, it's a DNS/MagicDNS issue, not a share problem. This is a common snag in **synology tailscale remote access** setups.

## Verification Test

After adjusting the firewall or re-enabling SMB, repeat the port test and then mount the share by IP. Success: the share opens and DSM loads. If it still fails, reboot the NAS once and retest.

## Rollback Note

If changes make things worse, revert the firewall rule you added and restart Tailscale on the NAS. Then reapply one change at a time so you can isolate the cause.

Still stuck after these checks?

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
