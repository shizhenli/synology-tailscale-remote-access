---
title: "Tailscale Shows Connected but Synology NAS Share Is Unreachable on Cellular"
description: "Fix Tailscale connected but Synology NAS share unreachable on cellular. Step-by-step checks for SMB/DSM access, firewall, and verification."
seo_title: "Synology Remote Access Cellular: Fix Tailscale Connected but NAS Share Unreachable"
seo_description: "Fix Tailscale connected but Synology NAS share unreachable on cellular. Step-by-step checks for SMB/DSM access, firewall, and verification."
keywords:
  - "synology remote access cellular"
date: 2026-11-22T09:08:28+08:00
publishDate: 2026-11-22T09:08:28+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're on cellular, Tailscale says connected, but the Synology NAS share won't open. Let's walk through this calmly.

## Check Tailscale IP and Ping
First, confirm you're using the NAS's Tailscale IP (100.x.x.x), not its LAN IP. On your remote device, open Tailscale and note the NAS address. Then ping that IP. If ping succeeds, the tunnel is up; if it fails, Tailscale isn't routing—recheck that the NAS is online and Tailscale is running.

## Verify SMB and DSM Ports
If ping works but SMB fails, test DSM in a browser: https://100.x.x.x:5001. If DSM loads but SMB doesn't, the issue is SMB-specific. On the NAS, open Control Panel > File Services > SMB and confirm it's enabled. Also check Control Panel > Security > Firewall: ensure the Tailscale interface (tailscale0) is allowed, or temporarily disable the firewall to test. If DSM also fails, check that the Tailscale IP is bound to the correct interface.

## Test from Another Device
Try accessing the share from a second device on the same Tailscale network. If it works there, the problem is your cellular device's SMB client or DNS. If it fails everywhere, the NAS side is blocking.

## Rollback and Recovery
If you changed firewall rules or SMB settings, revert them before further testing. Keep a note of original settings so you can restore quickly.

## Verification
After adjustments, disconnect and reconnect Tailscale on your phone, then open the share again. Success means the share mounts and DSM loads over the Tailscale IP.

If it still fails,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your ping and port test results. For **synology remote access cellular** issues, this focused check usually isolates the block.
