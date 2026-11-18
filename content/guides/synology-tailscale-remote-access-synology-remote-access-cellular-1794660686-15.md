---
title: "Synology Remote Access Cellular: Tailscale Shows Connected but SMB and DSM Are Unreachable"
description: "Tailscale says connected but you can't reach SMB or DSM on your Synology NAS over cellular? Follow these targeted checks to restore access."
seo_title: "Synology Remote Access Cellular: Fix Tailscale Connected but NAS Unreachable"
seo_description: "Tailscale says connected but you can't reach SMB or DSM on your Synology NAS over cellular? Follow these targeted checks to restore access."
keywords:
  - "synology remote access cellular"
date: 2026-11-18T09:08:29+08:00
publishDate: 2026-11-18T09:08:29+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're on cellular, Tailscale reports connected, yet SMB and DSM won't load on your Synology NAS.

## Confirm the connection is actually usable

On the remote device, open Tailscale and note the NAS's Tailscale IP (100.x.x.x). Ping it. If ping fails, the tunnel is not passing traffic even though the status says connected. If ping succeeds but SMB and DSM fail, the tunnel is fine and the problem is on the NAS side.

## Check the NAS Tailscale interface and services

On the NAS, open Package Center and confirm Tailscale is running. Then check Control Panel > Network > Network Interface: the Tailscale interface should be up with the expected 100.x address. Next, verify SMB is enabled under Control Panel > File Services > SMB, and DSM is reachable locally at https://NAS-LAN-IP:5001. If DSM works locally but not over Tailscale, the tunnel or firewall is dropping the remote path.

## Rule out firewall and subnet conflicts

In Control Panel > Security > Firewall, temporarily disable the active profile and retest. If access returns, re-enable it and add allow rules for the Tailscale interface and ports 445 and 5001. Also confirm the NAS LAN subnet does not overlap the cellular device's local subnet, which can silently break routing.

## Verification test

From the remote device, connect to smb://100.x.x.x and open https://100.x.x.x:5001. Both should load. If only one works, focus on that service's port and firewall rule.

## Rollback note

If disabling the firewall did not help, re-enable it before continuing so you don't leave the NAS exposed. If the NAS becomes unreachable entirely, reconnect it to your LAN and review changes locally.

For a guided diagnosis of your **synology remote access cellular** setup,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
