---
title: "Mobile Data Fails via Tailscale Exit Node: Diagnose the Synology NAS Path"
description: "Troubleshoot Synology Tailscale mobile data failures when using an exit node. Check routing, DNS, and firewall rules with exact result interpretation and a verification test."
seo_title: "Synology Tailscale Mobile Data: Fix Exit Node Failure"
seo_description: "Troubleshoot Synology Tailscale mobile data failures when using an exit node. Check routing, DNS, and firewall rules with exact result interpretation and a verification test."
keywords:
  - "synology tailscale mobile data"
date: 2026-11-21T16:14:26+08:00
publishDate: 2026-11-21T16:14:26+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You are trying to reach your Synology NAS through Tailscale, but remote access fails when your phone switches to mobile data or uses an exit node.

## Confirm the exit node is actually selected
On the phone, open Tailscale and check the exit node status. If it shows none, select your Synology NAS as the exit node. Result interpretation: if the toggle stays off or reverts, the NAS is not advertising itself correctly. On the NAS, run `tailscale status` and confirm the exit node flag appears. If it does not, re-enable exit node advertisement in the Tailscale admin console.

## Test basic reachability without the exit node
Disable the exit node on the phone and switch to mobile data only. Open a browser and navigate to your NAS using its Tailscale IP (100.x.x.x). If the page loads, the base tunnel works and the failure is exit-node-specific. If it does not load, the problem is the mobile-data path itself, not the exit node.

## Check DNS and routing on mobile data
With mobile data active, run `nslookup your-nas-name` from a terminal app. If it returns a public IP instead of the Tailscale 100.x address, MagicDNS is not applying. Disable any private DNS or VPN override on the phone. Then confirm the NAS firewall allows the Tailscale interface. A blocked UDP port 41641 will cause intermittent failures on cellular networks.

## Verification test
Re-enable the exit node, stay on mobile data, and load the NAS login page. Success means the page renders within ten seconds. Repeat once after toggling airplane mode to confirm stability.

## Rollback note
If changes break local access, disable the exit node on the phone and revert the last firewall rule. Your **synology tailscale mobile data** path should then return to its previous state. For guided diagnosis,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
