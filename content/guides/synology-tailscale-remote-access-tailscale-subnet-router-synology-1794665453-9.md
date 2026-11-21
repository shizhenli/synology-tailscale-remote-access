---
title: "Tailscale Subnet Router on Synology: LAN Devices Still Unreachable?"
description: "Remote client can't reach LAN devices through your Synology subnet router? Follow these checks to diagnose and restore LAN exposure without guesswork."
seo_title: "Tailscale Subnet Router Synology: Fix LAN Not Exposed"
seo_description: "Remote client can't reach LAN devices through your Synology subnet router? Follow these checks to diagnose and restore LAN exposure without guesswork."
keywords:
  - "tailscale subnet router synology"
date: 2026-11-21T16:14:26+08:00
publishDate: 2026-11-21T16:14:26+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

A remote client cannot reach LAN devices through the Synology subnet router, and you are stuck before completing a controlled diagnosis. Let's walk through it step by step.

## Confirm the subnet router is actually advertising the LAN

On the Synology, open the Tailscale package and check the advertised routes. The LAN subnet must appear exactly as the router advertises it, for example 192.168.1.0/24. If the field is empty or contains the wrong interface, the router is not exposing the LAN. Add the correct subnet and save. Then, in the Tailscale admin console, open the Synology machine and approve the subnet route. If it shows as unapproved, the remote client will never reach LAN devices.

## Check client-side routing and ACLs

On the remote client, run `tailscale status` and look for the Synology peer. Then test with `ping 192.168.1.10` (replace with a real LAN device IP). If the ping fails, run `tailscale ping 192.168.1.10`. A successful Tailscale ping but failed regular ping points to missing route acceptance on the client. Also verify the ACL grants access to the subnet, not just the Synology node.

## Verification test

From the remote client, ping a LAN device and then open its web interface. If both succeed, the subnet router is working. If only the Synology is reachable, the route is still not approved or the client has not accepted the subnet.

## Rollback and recovery

If changes make things worse, remove the advertised subnet on the Synology and re-approve it in the admin console. This returns you to the previous state without disrupting direct Tailscale access to the NAS.

For a guided diagnosis of your **tailscale subnet router synology** setup,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
