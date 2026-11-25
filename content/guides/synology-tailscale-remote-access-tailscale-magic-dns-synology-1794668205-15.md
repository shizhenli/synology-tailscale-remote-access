---
title: "Tailscale MagicDNS Name Not Resolving on Synology: A Step-by-Step Diagnosis"
description: "Troubleshoot why your Synology NAS Tailscale name fails to resolve on a remote client. Step-by-step checks for MagicDNS, DNS settings, and verification."
seo_title: "Fix Tailscale MagicDNS Name Not Resolving on Synology NAS"
seo_description: "Troubleshoot why your Synology NAS Tailscale name fails to resolve on a remote client. Step-by-step checks for MagicDNS, DNS settings, and verification."
keywords:
  - "tailscale magic dns synology"
date: 2026-11-25T09:08:35+08:00
publishDate: 2026-11-25T09:08:35+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're connected to Tailscale, but when you try to reach your Synology NAS by its MagicDNS name on a remote client, the name simply doesn't resolve.

## Check the Tailscale status on both devices

On the NAS, open the Tailscale package or SSH and run `tailscale status`. Look for the line showing the NAS itself; it should list a MagicDNS name like `nas-name.tailnet-name.ts.net`. If that name is missing or shows as `-`, MagicDNS is not active on the tailnet. On the remote client, run the same command and confirm the NAS appears with the same MagicDNS name. If the client shows the NAS only by IP, the client isn't receiving MagicDNS records.

## Verify DNS settings in the Tailscale admin console

Log in to the Tailscale admin console and go to the DNS tab. Ensure MagicDNS is enabled. Check that the tailnet name matches what you expect. If you use an override DNS server, confirm it isn't blocking the `.ts.net` domain. Also verify that the NAS and client are both in the same tailnet and that no ACL rule prevents DNS resolution between them.

## Test resolution directly

On the remote client, run `nslookup nas-name.tailnet-name.ts.net` (or `dig`). A successful response should return the NAS's Tailscale IP (100.x.x.x). If it times out or returns NXDOMAIN, the client isn't using Tailscale's DNS. On Windows, check that the Tailscale network adapter has DNS set to the Tailscale resolver; on macOS or Linux, confirm the resolver is active. You can also try `ping nas-name.tailnet-name.ts.net` to see if it resolves to the correct IP.

## Rollback and recovery

If you changed DNS settings, revert them to the previous state before testing again. Restart the Tailscale service on both devices after any change. If the issue persists, re-authenticate the NAS in the Tailscale admin console. For a controlled diagnosis of your **tailscale magic dns synology** setup,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
