---
title: "Tailscale DNS Not Resolving: Fix the MagicDNS Name for Your Synology NAS (Step-by-Step)"
description: "Troubleshoot why your Synology NAS Tailscale name fails to resolve on a remote client. Step-by-step checks, exact result interpretation, and a verification test."
seo_title: "Tailscale DNS Not Resolving: Fix MagicDNS Name for Synology NAS"
seo_description: "Troubleshoot why your Synology NAS Tailscale name fails to resolve on a remote client. Step-by-step checks, exact result interpretation, and a verification test."
keywords:
  - "tailscale dns not resolving"
date: 2026-11-21T16:17:29+08:00
publishDate: 2026-11-21T16:17:29+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected to your Synology NAS via Tailscale, but the MagicDNS name won't resolve on your client, leaving you stuck before you can diagnose further.

## Check Tailscale Status on the NAS

SSH into the NAS or use the Tailscale package UI. Run `tailscale status` and look for the NAS's MagicDNS name (e.g., `nas.tailnet-name.ts.net`). If the name appears but the client can't resolve it, the issue is likely DNS configuration or propagation. If the name is missing, Tailscale isn't fully initialized—restart the Tailscale service on the NAS.

## Verify DNS Settings on the Client

On the remote client, run `tailscale dns status` (or check the Tailscale app's DNS tab). Confirm MagicDNS is enabled and the NAS's tailnet is listed. If MagicDNS is off, enable it. If it's on but the name still fails, flush the DNS cache: `sudo systemd-resolve --flush-caches` on Linux, `sudo dscacheutil -flushcache` on macOS, or `ipconfig /flushdns` on Windows. Then retry resolving the name with `nslookup nas.tailnet-name.ts.net`.

## Test Resolution and Connectivity

After flushing, run `ping nas.tailnet-name.ts.net`. A successful ping confirms resolution. If it fails, try the NAS's Tailscale IP directly (`ping 100.x.x.x`). If the IP works but the name doesn't, the problem is isolated to DNS—this is a classic **tailscale dns not resolving** symptom. Check that the client's Tailscale version is current and that no local DNS resolver (like Pi-hole) is intercepting queries.

## Rollback and Recovery

If changes worsen connectivity, revert to using the Tailscale IP for access. Disable any manual DNS overrides you added, restart Tailscale on both NAS and client, and re-enable MagicDNS. This restores the previous working state.

If the name still won't resolve after these steps,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your `tailscale status` and `tailscale dns status` outputs for a controlled diagnosis.
