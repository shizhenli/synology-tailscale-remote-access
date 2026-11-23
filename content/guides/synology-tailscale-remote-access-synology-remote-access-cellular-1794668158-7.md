---
title: "Synology Remote Access Cellular: Tailscale Connects but NAS Share Is Unreachable"
description: "Tailscale shows connected on your phone, but the Synology NAS share won't open over cellular? Follow these checks to diagnose SMB/DSM reachability and restore access."
seo_title: "Synology Remote Access Cellular: Fix Tailscale Connected but NAS Share Unreachable"
seo_description: "Tailscale shows connected on your phone, but the Synology NAS share won't open over cellular? Follow these checks to diagnose SMB/DSM reachability and restore access."
keywords:
  - "synology remote access cellular"
date: 2026-11-23T12:08:43+08:00
publishDate: 2026-11-23T12:08:43+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're on cellular, Tailscale says connected, but the Synology share or DSM page won't open. That's a common **synology remote access cellular** snag, and it's usually a reachability issue, not a Tailscale failure.

## Confirm what "connected" actually means

Open the Tailscale app on your phone and check the NAS peer. If it shows a green dot but no direct or relay path, tap the peer and note the address (100.x.x.x). Then try pinging that address from the phone's terminal app. A reply means the tunnel is fine; timeouts mean the NAS isn't advertising or the phone's route is stale. Toggle Tailscale off and on once, then retest.

## Check the NAS-side services

From a LAN device, confirm DSM is listening on 5000/5001 and SMB on 445. On the NAS, open Control Panel > Firewall and ensure the Tailscale interface (tailscale0) is allowed, not just the LAN. In Tailscale admin, verify the NAS is not set to "block incoming connections." If you use a subnet router, confirm the NAS's own Tailscale IP is reachable before testing LAN devices.

## Verification test

On the phone, browse to http://100.x.x.x:5000 (use the NAS Tailscale IP). If DSM loads, SMB should follow; if it doesn't, the block is on the NAS firewall or the Tailscale ACL. Re-enable one rule at a time and retest.

## Rollback note

If a firewall or ACL change breaks LAN access, revert the last rule and restart the Tailscale package on the NAS. Keep a LAN session open while testing so you can undo changes.

Still stuck after these checks?

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
