---
title: "Fix Tailscale MagicDNS Name Not Resolving for Synology NAS"
description: "Troubleshoot Tailscale MagicDNS name resolution failure for your Synology NAS. Step-by-step checks to restore remote access using the MagicDNS hostname."
seo_title: "Tailscale MagicDNS Synology: Fix Name Resolution Failure"
seo_description: "Troubleshoot Tailscale MagicDNS name resolution failure for your Synology NAS. Step-by-step checks to restore remote access using the MagicDNS hostname."
keywords:
  - "tailscale magic dns synology"
date: 2026-11-21T16:11:24+08:00
publishDate: 2026-11-21T16:11:24+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected to your Synology NAS via Tailscale, but the MagicDNS name fails to resolve on your client. This stops you from reaching the NAS by its Tailscale hostname.

## Check Tailscale Status and MagicDNS Settings

First, verify that Tailscale is running on both the NAS and the client. On the NAS, open the Tailscale package and confirm it shows "Connected." On the client, check the Tailscale status. Next, ensure MagicDNS is enabled in your Tailscale admin console under DNS settings. If it's off, turn it on and save. Then, on the client, run `tailscale status` and look for the NAS entry. If the NAS appears with a hostname like `nas-name.tailnet-name.ts.net`, MagicDNS is active. If it shows only an IP, MagicDNS may not be applied.

## Test DNS Resolution

On the client, use `nslookup nas-name.tailnet-name.ts.net` or `ping nas-name.tailnet-name.ts.net`. If it resolves to the NAS's Tailscale IP (100.x.x.x), DNS works. If it fails, check the client's DNS settings: Tailscale should set the DNS server to 100.100.100.100. Run `tailscale dns status` on the client to see if MagicDNS is handling queries. If not, restart Tailscale on the client.

## Verify with Direct IP

Try accessing the NAS by its Tailscale IP (e.g., `ping 100.x.x.x`). If that works but the name doesn't, the issue is DNS-specific. This confirms the **tailscale magic dns synology** name resolution is the problem, not connectivity.

## Rollback and Recovery

If changes to DNS settings cause issues, revert them: disable MagicDNS, save, then re-enable. On the NAS, restart the Tailscale package. If problems persist, consult

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).

After fixing, test again with `nslookup` to confirm the name resolves. This ensures reliable remote access using the MagicDNS name.
