---
title: "Subnet Router Does Not Expose the LAN: Fixing the Synology Tailscale Route"
description: "Remote client can't reach LAN devices through your Synology Tailscale subnet router? Follow these checks to verify advertised routes, client approval, and forwarding."
seo_title: "Tailscale Subnet Router Synology: LAN Not Exposed Fix"
seo_description: "Remote client can't reach LAN devices through your Synology Tailscale subnet router? Follow these checks to verify advertised routes, client approval, and forwarding."
keywords:
  - "tailscale subnet router synology"
date: 2026-11-16T11:08:18+08:00
publishDate: 2026-11-16T11:08:18+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

A remote client can see your Synology NAS over Tailscale but cannot reach other LAN devices behind it, and you are stuck at the subnet router does not expose the LAN step before completing a controlled diagnosis.

## Confirm the route is actually advertised

Open the Tailscale admin console and select the Synology machine. Under Subnets, confirm the LAN prefix (for example 192.168.1.0/24) is listed. If it is missing, re-run the subnet router command with the correct CIDR and restart Tailscale. If it is listed but greyed out, the route is advertised but not approved.

## Approve the route and check client state

On the admin console, approve the subnet route for the Synology node. Then, on the remote client, run tailscale status and look for the Synology peer. The route should appear as accepted. If the client shows the route as offered but not accepted, run tailscale up --accept-routes on that client. This is the most common reason a **tailscale subnet router synology** setup appears to expose nothing.

## Verify IP forwarding on the NAS

SSH into the Synology and run sysctl net.ipv4.ip_forward. The expected result is net.ipv4.ip_forward = 1. If it returns 0, enable forwarding in the Tailscale startup configuration or via the Synology task scheduler, then restart the package.

## Run a verification test

From the remote client, ping a LAN device IP (not the NAS). A reply confirms the route works. If ping fails but the NAS responds, recheck the advertised CIDR against the actual LAN subnet.

## Rollback note

If changes break NAS access, remove the subnet route in the admin console and restart Tailscale on the Synology to restore the previous state. For guided diagnosis,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
