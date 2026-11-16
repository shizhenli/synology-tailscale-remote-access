---
title: "Synology Tailscale Not Connecting: When Tailscale Shows Connected but the NAS Share Is Unreachable"
description: "Tailscale shows connected but your Synology NAS share is unreachable? Follow these practical checks to diagnose SMB and DSM access over Tailscale and restore remote access."
seo_title: "Fix Synology Tailscale Not Connecting: Connected but NAS Share Unreachable"
seo_description: "Tailscale shows connected but your Synology NAS share is unreachable? Follow these practical checks to diagnose SMB and DSM access over Tailscale and restore remote access."
keywords:
  - "synology tailscale not connecting"
date: 2026-11-16T09:08:14+08:00
publishDate: 2026-11-16T09:08:14+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You are remotely connected through Tailscale, the client says connected, but SMB or DSM on your Synology NAS will not open. This is the classic **synology tailscale not connecting** symptom where the tunnel is up but the share is unreachable, so work through the checks below before changing anything.

## Confirm the Tailscale IP and ping path

Open the Tailscale client on the remote device and note the NAS Tailscale IP (usually 100.x.x.x). Ping that address. If ping fails, the tunnel is not actually passing traffic; if ping succeeds but SMB fails, the problem is above the network layer. Also confirm the NAS shows as connected in the Tailscale admin console, not just the client.

## Check the NAS service and firewall

On the NAS, open Control Panel and verify SMB is enabled and DSM is listening on the expected ports (5000/5001 for DSM, 445 for SMB). Then check the Synology firewall: if it is enabled, confirm the Tailscale interface is allowed, or temporarily disable the firewall rule set to test. A firewall that permits only the LAN interface will drop Tailscale traffic even though the tunnel is connected.

## Verify SMB binding and share permissions

In File Services, confirm SMB is bound to all interfaces or includes the Tailscale interface. Then check that your user account has permission to the specific shared folder. A connected tunnel with no share permission looks identical to a broken connection.

## Run the verification test

From the remote device, run `ping <NAS-Tailscale-IP>` and then open `\\<NAS-Tailscale-IP>\share` (Windows) or `smb://<NAS-Tailscale-IP>/share` (macOS). If ping succeeds and the share opens, access is restored. If ping succeeds but the share still fails, recheck the firewall and SMB binding.

## Rollback note

If a firewall or SMB change makes things worse, re-enable the previous rule or binding immediately and retest local LAN access before continuing.

If these checks do not restore the share,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
