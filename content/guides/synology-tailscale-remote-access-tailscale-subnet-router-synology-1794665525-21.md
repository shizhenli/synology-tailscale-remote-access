---
title: "Tailscale Subnet Router on Synology: When the LAN Is Not Exposed"
description: "Step-by-step diagnosis for when a remote client cannot reach LAN devices through a Synology Tailscale subnet router, including checks, verification, and rollback."
seo_title: "Fix Tailscale Subnet Router on Synology Not Exposing LAN"
seo_description: "Step-by-step diagnosis for when a remote client cannot reach LAN devices through a Synology Tailscale subnet router, including checks, verification, and rollback."
keywords:
  - "tailscale subnet router synology"
date: 2026-11-22T11:08:41+08:00
publishDate: 2026-11-22T11:08:41+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You set up the Synology as a Tailscale subnet router, but a remote client still cannot reach LAN devices, and you are stuck before a controlled diagnosis. Here is how to isolate the failure without guessing.

## Confirm the subnet router is actually advertised

On the Synology, open the Tailscale package and check that the subnet route (for example, 192.168.1.0/24) is listed and approved. On the remote client, run `tailscale status` and look for the Synology node showing the subnet route. If the route is missing or unapproved, the LAN is not being advertised at all; approve it in the Tailscale admin console under the machine's route settings.

## Check IP forwarding and client routing

On the Synology, verify IP forwarding is enabled: `sysctl net.ipv4.ip_forward` should return 1. If it returns 0, the subnet router cannot forward traffic. On the remote client, confirm the LAN subnet is in its routing table (`ip route` or `route print`). If the route is absent, the client is not accepting the advertised subnet; re-authenticate the client or restart Tailscale.

## Rule out firewall and ACL blocks

Temporarily disable the Synology firewall and retest. If access returns, the firewall is dropping forwarded traffic; re-enable it and allow the Tailscale interface and LAN subnet. Also check Tailscale ACLs for a rule that permits the remote user to the subnet.

## Verification test

From the remote client, ping a LAN device by IP, then open its web interface. A successful ping and page load confirm the subnet route works. If ping fails but the Synology itself responds, the issue is forwarding or firewall, not Tailscale connectivity.

## Rollback note

If changes break access, revert the firewall rule and re-approve the original subnet route. For guided diagnosis of your **tailscale subnet router synology** setup,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
