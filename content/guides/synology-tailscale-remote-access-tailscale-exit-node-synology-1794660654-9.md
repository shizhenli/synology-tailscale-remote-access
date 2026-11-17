---
title: "Tailscale Exit Node on Synology: Fix Mobile Data and Exit Node Failures"
description: "Troubleshoot Tailscale exit node and mobile data failures on Synology NAS. Step-by-step checks for IP forwarding, ACLs, and routing to restore remote access."
seo_title: "Tailscale Exit Node Synology: Fix Mobile Data & Exit Node Failures"
seo_description: "Troubleshoot Tailscale exit node and mobile data failures on Synology NAS. Step-by-step checks for IP forwarding, ACLs, and routing to restore remote access."
keywords:
  - "tailscale exit node synology"
date: 2026-11-17T08:16:31+08:00
publishDate: 2026-11-17T08:16:31+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're trying to reach your Synology NAS remotely, but when your phone switches to mobile data or you enable an exit node, the connection drops and you can't complete a controlled diagnosis.

## Check IP Forwarding on the Synology

SSH into the NAS and run:

```
sysctl net.ipv4.ip_forward
```

If it returns `0`, forwarding is off. Enable it with `sudo sysctl -w net.ipv4.ip_forward=1` and make it persistent by adding `net.ipv4.ip_forward=1` to `/etc/sysctl.conf`. Without this, the **tailscale exit node synology** setup cannot route traffic.

## Verify Exit Node Advertise and Approval

On the NAS, run `tailscale status` and confirm the exit node shows `offers exit node`. In the Tailscale admin console, go to Machines, select your Synology, and ensure "Use as exit node" is approved. If it's pending, approve it and wait 30 seconds.

## Test the Mobile Data Path

Disable Wi-Fi on your phone, enable mobile data, and open the Tailscale app. Toggle the exit node on. Run `tailscale ping <NAS-Tailscale-IP>` from another device on your tailnet. A successful reply means routing works; a timeout means the exit node isn't forwarding.

## Check ACLs and Firewall

In the admin console, review Access Controls. Ensure your phone's user has `autogroup:internet` access when using an exit node. On the Synology, confirm the firewall allows UDP 41641 and the Tailscale interface.

## Verification Test

From your phone on mobile data with the exit node active, browse to `http://<NAS-Tailscale-IP>:5000`. If DSM loads, the path is restored. If not, run `tailscale netcheck` on the NAS and note any DERP relay issues.

## Rollback Note

If changes break local access, disable the exit node in the Tailscale app, revert `sysctl` changes, and restart the Tailscale package. For guided diagnosis,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
