---
title: "Tailscale Subnet Router on Synology: When the LAN Stays Hidden"
description: "A remote client can't reach LAN devices through your Synology subnet router. Follow these checks to diagnose why the LAN isn't exposed and restore access."
seo_title: "Fix Tailscale Subnet Router Synology LAN Not Exposed"
seo_description: "A remote client can't reach LAN devices through your Synology subnet router. Follow these checks to diagnose why the LAN isn't exposed and restore access."
keywords:
  - "tailscale subnet router synology"
date: 2026-11-28T21:55:54+08:00
publishDate: 2026-11-28T21:55:54+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You set up your Synology NAS as a Tailscale subnet router, but a remote client still cannot reach devices on the LAN behind it. This is a common stopping point, and the fix usually comes down to one of three things: the route isn't advertised, the client hasn't accepted it, or the NAS isn't forwarding traffic.

## Confirm the subnet is actually advertised

On the NAS, run `tailscale status --json` and look for the `AdvertisedRoutes` field. It should list your LAN subnet, for example `192.168.1.0/24`. If it's empty, the `--advertise-routes` flag didn't take effect. Re-run the subnet router command with the correct CIDR, then restart the Tailscale service.

## Check that the client accepted the route

On the remote client, run `tailscale status` and check whether the subnet appears under the NAS peer. If it's missing, the route wasn't approved. In the Tailscale admin console, open the NAS machine and approve the subnet under its route settings. Without this approval, the route stays inactive even though the NAS advertises it.

## Verify IP forwarding on the NAS

SSH into the NAS and run `sysctl net.ipv4.ip_forward`. The result must be `1`. If it returns `0`, enable forwarding with `sysctl -w net.ipv4.ip_forward=1` and persist it in `/etc/sysctl.conf`. This is the most frequent reason a **tailscale subnet router synology** setup silently drops LAN traffic.

## Run a verification test

From the remote client, ping a LAN device by IP, not hostname. A successful reply confirms routing works. If ping fails, run `tailscale ping <NAS-tailscale-IP>` to confirm the tunnel itself is healthy.

## Rollback note

If changes break connectivity, remove the advertised route with `tailscale up --advertise-routes=` and restart the service to return to the previous state.

If the route still won't expose the LAN,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your route output and client status.
