---
title: "Tailscale MagicDNS Name Not Resolving on Synology: A Step-by-Step Fix"
description: "Troubleshoot why your Synology NAS Tailscale MagicDNS name fails to resolve on a remote client. Follow these checks to restore access."
seo_title: "Fix Tailscale MagicDNS Not Resolving for Synology NAS"
seo_description: "Troubleshoot why your Synology NAS Tailscale MagicDNS name fails to resolve on a remote client. Follow these checks to restore access."
keywords:
  - "tailscale magic dns synology"
date: 2026-11-23T10:08:35+08:00
publishDate: 2026-11-23T10:08:35+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected via Tailscale, but the MagicDNS name for your Synology NAS won't resolve on your client device.

## Check Tailscale Status on the NAS

First, confirm Tailscale is running on the NAS. Open SSH or the Synology Terminal and run:

```
tailscale status
```

Look for your NAS's hostname and its 100.x.y.z IP. If the status shows "stopped" or no IP, start the service with `sudo tailscale up`. If it's running, note the exact MagicDNS name (e.g., `nas-name.tailnet-name.ts.net`).

## Verify MagicDNS is Enabled in Your Tailnet

On the Tailscale admin console, go to DNS settings. Ensure MagicDNS is toggled on. If it's off, enable it and save. This is the most common cause of a **tailscale magic dns synology** name failing to resolve.

## Check Client-Side DNS Configuration

On the remote client, run `tailscale status` and confirm it shows the NAS. Then test resolution:

```
nslookup nas-name.tailnet-name.ts.net
```

If it returns the 100.x.y.z IP, DNS is working. If not, check that the client is using Tailscale's DNS. On Windows, run `ipconfig /all` and look for Tailscale's DNS suffix. On macOS/Linux, check `/etc/resolv.conf` for a Tailscale nameserver (100.100.100.100).

## Verification Test

After enabling MagicDNS and confirming client DNS, run the nslookup again. A successful response with the NAS's Tailscale IP means the name resolves. Then try accessing the NAS via its MagicDNS name in a browser or file manager.

## Rollback and Recovery

If changes cause connectivity issues, revert by disabling MagicDNS in the admin console and restarting Tailscale on the NAS (`sudo tailscale down && sudo tailscale up`). This restores previous behavior. For persistent problems,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
