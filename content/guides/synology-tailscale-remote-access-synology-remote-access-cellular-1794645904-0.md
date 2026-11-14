---
title: "Synology Remote Access Cellular: Fix Tailscale Connected but NAS Share Unreachable"
description: "Troubleshoot Tailscale showing connected but SMB or DSM unreachable on Synology NAS over cellular. Step-by-step checks and verification."
seo_title: "Synology Remote Access Cellular: Tailscale Connected but NAS Unreachable"
seo_description: "Troubleshoot Tailscale showing connected but SMB or DSM unreachable on Synology NAS over cellular. Step-by-step checks and verification."
keywords:
  - "synology remote access cellular"
date: 2026-11-14T19:47:13+08:00
publishDate: 2026-11-14T19:47:13+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're on cellular, Tailscale says connected, but your Synology NAS share won't open—let's walk through this together.

## Confirm Tailscale Status and IP

On the remote device, open the Tailscale app and note the NAS's Tailscale IP (100.x.x.x). On the NAS, open Package Center > Tailscale and verify it shows connected with the same IP. If the IP differs, log out and back in on both ends. A connected status with an unreachable share often means the device is using the wrong path.

## Test Basic Reachability

From the remote device, ping the NAS Tailscale IP. If ping fails, Tailscale isn't routing traffic—check that the remote device's Tailscale is active and not in "exit node" mode incorrectly. If ping succeeds but SMB fails, the issue is service-level, not network-level.

## Check DSM and SMB Services

On the NAS, open Control Panel > File Services and confirm SMB is enabled. Then Control Panel > Terminal & SNMP and ensure SSH is on for testing. From the remote device, try accessing DSM via https://[Tailscale-IP]:5001. If DSM loads but SMB doesn't, SMB is blocked or misconfigured. If neither loads, the NAS firewall may be dropping Tailscale traffic—check Control Panel > Security > Firewall and allow the Tailscale interface.

## Verification Test

On the remote device, run: smbclient -L //[Tailscale-IP] -U yourusername (or use Finder/Explorer to connect to smb://[Tailscale-IP]). A successful list of shares confirms the fix. If it fails, note the exact error.

## Rollback Note

If changes break local access, disable the firewall rule or revert SMB settings in Control Panel. Keep a local admin session open before editing.

For persistent cases, **synology remote access cellular** setups often need Tailscale ACL checks—verify your tailnet policy allows the remote device to reach the NAS on ports 445 and 5001. If stuck,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
