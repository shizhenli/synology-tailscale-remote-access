---
title: "Synology Tailscale Connected but NAS Share Unreachable: A Focused Fix"
description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow these focused checks to diagnose and restore access."
seo_title: "Synology Tailscale Connected but NAS Share Unreachable: Fix SMB/DSM Access"
seo_description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow these focused checks to diagnose and restore access."
keywords:
  - "synology tailscale remote access"
date: 2026-11-24T09:08:32+08:00
publishDate: 2026-11-24T09:08:32+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected through Tailscale, it says connected, but you can't open your Synology NAS share or DSM interface. Let's walk through a controlled diagnosis.

## Check the Tailscale IP and Ping
First, confirm you're using the correct Tailscale IP. On your remote device, open Tailscale and note the NAS's 100.x.y.z address. Then ping that IP. If ping succeeds, the tunnel is up. If it fails, Tailscale is connected but not routing to the NAS—check that the NAS Tailscale client is running and logged in.

## Verify SMB and DSM Ports
Next, test the actual services. On Windows, run `Test-NetConnection <tailscale-ip> -Port 445` for SMB, or `-Port 5000` for DSM. On macOS/Linux, use `nc -zv <tailscale-ip> 445`. A successful connection means the service is listening; a timeout or refusal points to a firewall or service issue on the NAS.

## Inspect Synology Firewall Rules
Log into DSM locally (or via another working path) and open Control Panel > Security > Firewall. Ensure the Tailscale interface (usually `tailscale0`) is allowed, or that rules permit the 100.x.y.z subnet. A common mistake is blocking the Tailscale subnet while allowing only the LAN. Also confirm SMB is enabled under File Services.

## Verification Test
After adjusting firewall rules, retry the port test. Then attempt to map the share: `\\<tailscale-ip>\sharename` on Windows, or `smb://<tailscale-ip>/sharename` on macOS. If it opens, access is restored.

## Rollback Note
If changes break local access, revert the firewall rule you added. Keep a note of the original settings.

For persistent issues with **synology tailscale remote access**,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
