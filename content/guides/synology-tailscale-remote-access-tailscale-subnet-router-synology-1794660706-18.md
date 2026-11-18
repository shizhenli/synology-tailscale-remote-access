---
title: "Tailscale Subnet Router on Synology: LAN Devices Unreachable After Setup"
description: "Remote client can't reach LAN devices through your Synology subnet router? Follow these checks to diagnose and restore access."
seo_title: "Fix Tailscale Subnet Router on Synology Not Exposing LAN"
seo_description: "Remote client can't reach LAN devices through your Synology subnet router? Follow these checks to diagnose and restore access."
keywords:
  - "tailscale subnet router synology"
date: 2026-11-18T12:11:18+08:00
publishDate: 2026-11-18T12:11:18+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You've set up your Synology NAS as a Tailscale subnet router, but remote clients still can't reach LAN devices, and you're stuck before completing a controlled diagnosis.

## Check the subnet router advertisement

On the Synology, open the Tailscale package and confirm that the correct LAN subnet (for example, 192.168.1.0/24) is advertised. If it's missing, add it and restart the service. Then, from a remote client, run `tailscale status` and look for the Synology node. If the subnet appears under "Offers" but not "Routes," the advertisement isn't being accepted.

## Approve the subnet routes

In the Tailscale admin console, go to the Machines page and find your Synology. Under "Subnet routes," ensure the LAN subnet is approved. If it shows "Pending approval," click to approve it. Without approval, the **tailscale subnet router synology** won't expose the LAN to remote clients.

## Verify client-side routing

On the remote client, run `tailscale ip -4` to confirm it has a Tailscale IP. Then try pinging a LAN device by its IP (e.g., `ping 192.168.1.50`). If the ping fails, check that the client's Tailscale is up to date and that no local firewall blocks the Tailscale interface. Also, ensure the Synology's firewall allows forwarded traffic on the LAN interface.

## Test with a direct connection

From the remote client, attempt to access a LAN device's web interface or share. If it times out, the subnet route isn't working. If it works, the issue may be DNS or application-specific.

## Rollback and recovery

If changes cause loss of connectivity, revert to the previous subnet advertisement and re-approve routes. Restart Tailscale on both Synology and client.

If the problem persists,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) for a guided diagnosis.
