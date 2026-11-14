---
title: "Tailscale Subnet Router on Synology: When LAN Devices Stay Unreachable"
description: "Troubleshoot a Synology Tailscale subnet router that won't expose LAN devices: verify advertised routes, approve them in the admin console, enable IP forwarding, and test from a remote client."
seo_title: "Fix Tailscale Subnet Router Synology Not Exposing LAN"
seo_description: "Troubleshoot a Synology Tailscale subnet router that won't expose LAN devices: verify advertised routes, approve them in the admin console, enable IP forwarding, and test from a remote client."
keywords:
  - "tailscale subnet router synology"
date: 2026-11-14T19:47:13+08:00
publishDate: 2026-11-14T19:47:13+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You set up the Synology as a subnet router so a remote client could reach LAN devices, but those devices still don't respond. Here is a controlled way to find the break.

## Confirm the route is advertised and approved
On the Synology, open Tailscale and check that the subnet route (for example, 192.168.1.0/24) is listed as advertised. Then sign in to the Tailscale admin console, open the Synology machine, and confirm the route shows as approved, not pending. If it is pending, approve it. If no route appears, re-enter the LAN CIDR exactly as the Synology sees it.

## Verify IP forwarding on the Synology
SSH into the NAS and run: sysctl net.ipv4.ip_forward. The expected result is net.ipv4.ip_forward = 1. If it returns 0, forwarding is off and the subnet router cannot pass traffic. Enable it temporarily with sudo sysctl -w net.ipv4.ip_forward=1, then retest. For a persistent fix, add the setting through the Synology boot task method you already use.

## Check the client's view of the route
On the remote client, run tailscale status and look for the Synology node. Then run tailscale ping <LAN-device-IP>. A reply confirms the tunnel path; a timeout points to the route or forwarding issue above. Also confirm the client has not enabled an exit node that overrides local subnet routing.

## Verification test
From the remote client, ping a LAN device by IP, then open its web interface or share. Success means the route is approved and forwarding works.

## Rollback note
If changes make things worse, revert the sysctl value and remove the advertised route in Tailscale, then re-approve only after confirming the LAN CIDR. For a guided review of your **tailscale subnet router synology** setup,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
