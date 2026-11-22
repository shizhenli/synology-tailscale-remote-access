---
title: "Synology Tailscale Connected but SMB Share Unreachable: A Step-by-Step Diagnosis"
description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow this controlled diagnosis to restore synology tailscale remote access."
seo_title: "Synology Tailscale Remote Access: Fix SMB/DSM Unreachable When Connected"
seo_description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow this controlled diagnosis to restore synology tailscale remote access."
keywords:
  - "synology tailscale remote access"
date: 2026-11-22T10:08:36+08:00
publishDate: 2026-11-22T10:08:36+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected through Tailscale, the status says connected, but you still can't open the NAS share or DSM—let's walk through this together.

## Confirm the Tailscale IP and ping the NAS

First, on the remote device, open Tailscale and note the NAS's Tailscale IP (100.x.x.x). Then run `ping 100.x.x.x`. If replies come back, the tunnel is healthy and the problem is above the network layer. If it times out, stop here: the Tailscale connection itself is not passing traffic, so check that the NAS Tailscale client is logged in and not paused.

## Test the service ports directly

Next, test SMB and DSM separately. On Windows, run `Test-NetConnection 100.x.x.x -Port 445`; on macOS/Linux, use `nc -vz 100.x.x.x 445`. Then repeat with port 5001 for DSM. A `TcpTestSucceeded : True` or `succeeded` means the port is open—so the share issue is likely credentials, DNS name, or client mapping. A timeout means the NAS firewall or the service itself is blocking that port over the Tailscale interface.

## Check Synology firewall and service binding

In DSM, open Control Panel > Security > Firewall and confirm the Tailscale interface (usually `tailscale0`) is allowed for SMB and DSM. Also verify File Services > SMB is enabled and that the shared folder permissions include your user. If you changed firewall rules, note the original settings so you can revert them if remote access worsens.

## Verification test

From the remote device, map the share using the Tailscale IP directly: `\\100.x.x.x\ShareName` on Windows, or `smb://100.x.x.x/ShareName` on macOS. If it opens, the hostname was the culprit; if not, revisit the port test results.

## Rollback note

If a firewall or service change breaks local LAN access, restore the previous rule set and re-enable the original SMB settings before continuing.

For a guided review of your **synology tailscale remote access** setup,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
