---
title: "Synology Tailscale Not Connecting: When Connected Status Lies and SMB Stays Silent"
description: "Tailscale shows connected but your Synology NAS share is unreachable? Follow these practical checks to diagnose SMB and DSM access over Tailscale and restore your remote connection."
seo_title: "Synology Tailscale Not Connecting: Fix Connected but SMB Unreachable"
seo_description: "Tailscale shows connected but your Synology NAS share is unreachable? Follow these practical checks to diagnose SMB and DSM access over Tailscale and restore your remote connection."
keywords:
  - "synology tailscale not connecting"
date: 2026-11-26T09:06:48+08:00
publishDate: 2026-11-26T09:06:48+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected through Tailscale, the status says connected, but you still can't open your Synology NAS share or DSM interface. This is a common **synology tailscale not connecting** symptom where the tunnel is up but traffic to SMB or DSM is blocked or misrouted.

## Check Tailscale IP and Ping

First, confirm the NAS Tailscale IP from the Tailscale admin console. On your remote device, open a terminal and ping that IP. If ping succeeds, the tunnel is healthy and the problem is above the network layer. If ping fails, the connection is not truly established—check that the NAS Tailscale package is running and logged in.

## Verify SMB and DSM Ports

Next, test the specific service. For SMB, run `nc -zv <tailscale-ip> 445` (or use Test-NetConnection on Windows). For DSM, test port 5001 (HTTPS) or 5000 (HTTP). A refused connection means the service is not listening on the Tailscale interface. In DSM, go to Control Panel > File Services > SMB and ensure it is enabled. Under Control Panel > Network > Network Interface, confirm the Tailscale interface is present and not firewalled.

## Check Synology Firewall Rules

If ports are filtered, inspect Control Panel > Security > Firewall. Create or adjust a rule to allow the Tailscale subnet (typically 100.64.0.0/10) on ports 445, 5000, and 5001. Apply the rule and retest.

## Verification Test

From the remote device, attempt to mount the share using the Tailscale IP: `\\<tailscale-ip>\share` on Windows or `smb://<tailscale-ip>/share` on macOS. If it mounts, the fix is confirmed. Also open `https://<tailscale-ip>:5001` to verify DSM loads.

## Rollback Note

If changes break local access, disable the new firewall rule and restart the Tailscale package on the NAS. Revert to the previous working configuration before further testing.

If these steps don't restore access,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) for a guided diagnosis.
