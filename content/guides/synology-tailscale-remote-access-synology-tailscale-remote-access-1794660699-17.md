---
title: "Synology Tailscale Connected but NAS Share Unreachable: A Step-by-Step Diagnosis"
description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow these practical checks to diagnose SMB and DSM access over Tailscale."
seo_title: "Synology Tailscale Remote Access: Fix Connected but Share Unreachable"
seo_description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow these practical checks to diagnose SMB and DSM access over Tailscale."
keywords:
  - "synology tailscale remote access"
date: 2026-11-18T11:18:13+08:00
publishDate: 2026-11-18T11:18:13+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected through Tailscale, the status says connected, but when you try to open the NAS share or DSM, nothing responds. This is a common fork in the road for **synology tailscale remote access**, and it usually comes down to one of three layers: the Tailscale IP path, the NAS service itself, or local firewall rules.

## Confirm You're Using the Tailscale IP

First, on the remote device, open a terminal and ping the NAS's Tailscale IP (100.x.x.x). If ping fails, Tailscale is connected but not routing to that peer. Check the Tailscale admin console: is the NAS online and are both devices in the same tailnet? If ping succeeds, move on.

## Test the Service Port Directly

Next, test SMB and DSM separately. From the remote device, run `nc -vz 100.x.x.x 445` for SMB and `nc -vz 100.x.x.x 5001` for DSM (or 5000 if HTTP). A successful connection means the port is open; a timeout means the NAS firewall or service is blocking it. On the NAS, open Control Panel > Security > Firewall and confirm the Tailscale interface (tailscale0) is allowed for SMB and DSM. Also verify SMB is enabled under File Services.

## Verification Test

Reconnect to the share using the Tailscale IP directly (for example, `\\100.x.x.x\share`) rather than the hostname. If that works, the issue was name resolution, not connectivity. If it still fails, review the firewall rule order and ensure the Tailscale subnet is permitted.

## Rollback Note

If you changed firewall rules or disabled services during diagnosis, revert them one at a time and retest after each change so you can identify which adjustment restored access.

If these steps don't resolve it,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your ping and port-test results.
