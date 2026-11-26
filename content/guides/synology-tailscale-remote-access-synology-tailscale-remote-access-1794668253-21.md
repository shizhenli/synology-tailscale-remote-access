---
title: "Synology Tailscale Connected but NAS Share Unreachable: Fix SMB and DSM Access"
description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow these practical checks to diagnose SMB and DSM access over Tailscale."
seo_title: "Synology Tailscale Connected but NAS Share Unreachable: Fix SMB and DSM Access"
seo_description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow these practical checks to diagnose SMB and DSM access over Tailscale."
keywords:
  - "synology tailscale remote access"
date: 2026-11-26T10:06:50+08:00
publishDate: 2026-11-26T10:06:50+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You are remotely connected through Tailscale, the status says connected, but your Synology NAS share or DSM page will not open. This is a common stop point in **synology tailscale remote access**, and it usually comes down to one of three layers: the Tailscale IP, the service itself, or the Synology firewall.

## Confirm the Tailscale IP and ping

On the remote device, open Tailscale and note the NAS Tailscale IP, usually 100.x.x.x. Run ping 100.x.x.x. If replies return, the tunnel is healthy and the problem is above the network layer. If ping fails, stop here and check that the NAS Tailscale package is running and logged in.

## Test the service directly

Try DSM first: open https://100.x.x.x:5001 in a browser. If DSM loads but SMB does not, the issue is SMB-specific. Next test SMB from the remote device: on Windows, run net use \\100.x.x.x\share /user:youruser. On macOS, use Finder > Go > Connect to Server and enter smb://100.x.x.x/share. A prompt for credentials means SMB is reachable; a timeout means it is blocked.

## Check the Synology firewall

In DSM, open Control Panel > Security > Firewall. If the firewall is enabled, confirm there is a rule allowing the Tailscale interface, typically tailscale0, or the 100.64.0.0/10 range. A missing rule drops SMB and DSM even though Tailscale reports connected. Add an allow rule for the Tailscale subnet, then re-test.

## Verification test

From the remote device, ping the NAS Tailscale IP, open DSM at https://100.x.x.x:5001, and mount the SMB share. All three should succeed. If DSM works but SMB still fails, check that SMB Service is enabled under Control Panel > File Services > SMB.

## Rollback note

If a firewall change makes things worse, disable the firewall temporarily to confirm the cause, then restore your previous rule set before making further changes. For a guided diagnosis,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
