---
title: "Tailscale Subnet Router on Synology: Why LAN Devices Remain Unreachable"
description: "A remote client cannot reach LAN devices through the Synology subnet router. Follow these checks to diagnose and restore LAN exposure."
seo_title: "Tailscale Subnet Router Synology: LAN Devices Unreachable Fix"
seo_description: "A remote client cannot reach LAN devices through the Synology subnet router. Follow these checks to diagnose and restore LAN exposure."
keywords:
  - "tailscale subnet router synology"
date: 2026-11-24T10:08:31+08:00
publishDate: 2026-11-24T10:08:31+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You are remotely connected to your Synology NAS via Tailscale, but when you try to reach a LAN device through the NAS acting as a subnet router, the connection fails. This means the subnet router is not exposing the LAN as expected. Let's diagnose step by step.

## Verify subnet routing is enabled and advertised
On the Synology, open the Tailscale package and confirm that subnet routes are enabled and the correct LAN subnet (e.g., 192.168.1.0/24) is advertised. If the subnet is missing or disabled, the router is not exposing the LAN. After enabling, note the exact subnet shown.

## Check route approval on the Tailscale admin console
Log in to the Tailscale admin console, find your Synology device, and ensure the advertised subnet route is approved. If it shows as unapproved, click to approve it. Without approval, remote clients cannot use the route.

## Confirm client is using the subnet router
On the remote client, run `tailscale status` and check that the Synology is listed as a relay or subnet router. Then test connectivity to a LAN device by pinging its IP (e.g., `ping 192.168.1.50`). If the ping fails, the route is not active. Also verify that the client's Tailscale IP is allowed by any ACLs.

## Verification test
From the remote client, attempt to access a LAN device's web interface or share using its LAN IP. If it loads, the subnet router is now exposing the LAN correctly. If not, recheck the above steps.

## Rollback and recovery
If changes cause loss of remote access to the NAS itself, disable subnet routing temporarily via the Synology Tailscale package or the admin console, then re-enable after correcting the configuration. This restores basic NAS connectivity.

If the issue persists, consult our guide on

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) for further assistance with your **tailscale subnet router synology** setup.
