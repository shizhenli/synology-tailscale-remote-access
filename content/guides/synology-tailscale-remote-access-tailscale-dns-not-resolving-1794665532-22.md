---
title: "MagicDNS Name Fails to Resolve on Remote Client: Tailscale DNS Fix for Synology"
description: "Troubleshoot why your Synology NAS Tailscale name fails to resolve on a remote client. Step-by-step checks for MagicDNS, DNS settings, and verification."
seo_title: "Tailscale DNS Not Resolving: Fix MagicDNS Name for Synology NAS"
seo_description: "Troubleshoot why your Synology NAS Tailscale name fails to resolve on a remote client. Step-by-step checks for MagicDNS, DNS settings, and verification."
keywords:
  - "tailscale dns not resolving"
date: 2026-11-22T12:08:54+08:00
publishDate: 2026-11-22T12:08:54+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected to your Synology NAS via Tailscale, but the MagicDNS name (like `nas.tailnet-name.ts.net`) won't resolve on your client, leaving you stuck before you can diagnose further.

## Check MagicDNS Status

On the remote client, open the Tailscale app and confirm MagicDNS is enabled. In the admin console, go to DNS and verify MagicDNS is on and your tailnet has a name. If disabled, enable it and retry. If enabled, proceed.

## Verify DNS Settings on the Client

Check the client's DNS configuration. Tailscale should set its DNS servers automatically. On Windows, run `ipconfig /all` and look for Tailscale's DNS suffix. On macOS or Linux, inspect `/etc/resolv.conf` or network settings. If Tailscale's DNS is missing, toggle Tailscale off and on to reapply settings.

## Test Resolution Directly

On the client, run `nslookup nas.tailnet-name.ts.net` (replace with your NAS name). If it returns the NAS Tailscale IP (100.x.x.x), resolution works—the issue may be app-specific. If it fails, try `ping nas.tailnet-name.ts.net`. A timeout suggests DNS is not reaching Tailscale's resolver.

## Check Synology Tailscale Settings

On the NAS, open Tailscale and ensure the machine name is set correctly and MagicDNS is active. Restart the Tailscale package if needed. Also confirm the NAS is online in the admin console.

## Rollback and Recovery

If changes break connectivity, disable MagicDNS, restart Tailscale on both devices, then re-enable. This often clears stale DNS state without losing access.

If the **tailscale dns not resolving** issue persists after these steps,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) for a guided diagnosis.
