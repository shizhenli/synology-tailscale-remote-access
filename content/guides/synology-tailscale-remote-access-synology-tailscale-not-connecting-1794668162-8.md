---
title: "Synology Tailscale Not Connecting: Fix SMB and DSM When Tailscale Shows Connected"
description: "Tailscale says connected but your Synology NAS share or DSM is unreachable? Follow these focused checks to restore SMB and DSM access over Tailscale."
seo_title: "Synology Tailscale Not Connecting: Fix SMB/DSM Unreachable"
seo_description: "Tailscale says connected but your Synology NAS share or DSM is unreachable? Follow these focused checks to restore SMB and DSM access over Tailscale."
keywords:
  - "synology tailscale not connecting"
date: 2026-11-24T08:08:33+08:00
publishDate: 2026-11-24T08:08:33+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected through Tailscale, the status shows connected, but SMB or DSM on your Synology NAS won't open. This is a common **synology tailscale not connecting** symptom, and it usually means the tunnel is up but something between Tailscale and the NAS service is blocking traffic. Let's diagnose it in order.

## Confirm the Tailscale IP and service port

On the remote device, open Tailscale and note the NAS's Tailscale IP (100.x.x.x). Then test the DSM port directly: in a browser, go to https://100.x.x.x:5001 (or http://100.x.x.x:5000). If DSM loads but SMB fails, the tunnel is fine and the issue is SMB-specific. If neither loads, continue below.

## Check the NAS firewall and Tailscale interface

In DSM, open Control Panel > Security > Firewall. Ensure the active profile allows the Tailscale interface (usually tailscale0) or the 100.64.0.0/10 range on ports 5000/5001 and 445. A rule that only allows your LAN subnet will silently drop Tailscale traffic. Apply the change and retest.

## Verify SMB is enabled and bound correctly

Open Control Panel > File Services > SMB. Confirm SMB is enabled and the workgroup matches your client. If you recently changed network settings, toggle SMB off, apply, then on again to rebind. On the remote device, clear cached credentials and reconnect to \\100.x.x.x\share.

## Verification test

From the remote device, run ping 100.x.x.x, then test the DSM port, then map the SMB share. All three should succeed. If ping works but ports fail, the firewall or service binding is still blocking.

## Rollback note

If a firewall or SMB change breaks local access, revert the last rule or toggle and reapply the previous SMB setting. Keep a DSM local login available before editing firewall rules.

If these steps don't restore access,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your Tailscale IP, DSM version, and firewall profile details.
