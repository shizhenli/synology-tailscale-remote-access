---
title: "Tailscale MagicDNS Fails on Synology: Diagnose the Name Resolution Gap"
description: "Remote client cannot resolve your Synology NAS Tailscale name. Step-by-step diagnosis for MagicDNS failure, with exact checks, verification, and rollback."
seo_title: "Fix tailscale dns not resolving on Synology NAS"
seo_description: "Remote client cannot resolve your Synology NAS Tailscale name. Step-by-step diagnosis for MagicDNS failure, with exact checks, verification, and rollback."
keywords:
  - "tailscale dns not resolving"
date: 2026-11-28T21:55:54+08:00
publishDate: 2026-11-28T21:55:54+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You are remotely connected to your Synology NAS through Tailscale, but when you try to reach it by its Tailscale name, the client reports that the host cannot be found. This is a **tailscale dns not resolving** symptom, and it usually means the MagicDNS name is not being handed to the client correctly. Work through the following checks in order.

## Confirm the client is using Tailscale DNS

On the remote client, run `tailscale status` and look for the NAS device. If it appears, run `tailscale dns status` (or `tailscale debug prefs` on older clients). The output should show a DNS nameserver and a search domain ending in `ts.net`. If the nameserver list is empty or points only to your local router, MagicDNS is not active on that client. Re-enable MagicDNS in the Tailscale admin console under DNS, then reconnect the client.

## Check the NAS Tailscale name and status

On the Synology, open the Tailscale package and confirm the device is logged in and shows as connected. Note the exact MagicDNS name shown. A common cause is a mismatch: the client is querying an old or renamed machine name. Compare the name in the admin console with the one you are using. If they differ, use the current name.

## Verify resolution directly

From the remote client, run `nslookup your-nas-name.your-tailnet.ts.net`. A correct result returns the NAS Tailscale IP (100.x.x.x). If it times out or returns a public address, the query is bypassing Tailscale DNS. Temporarily set the client's DNS to the Tailscale nameserver shown in `tailscale dns status` and retest.

## Rollback and recovery

If changes make things worse, revert the client DNS settings to automatic, restart the Tailscale service on both devices, and confirm the NAS still appears in `tailscale status`. If the name still fails after these steps,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your `tailscale dns status` output and the exact name you queried.
