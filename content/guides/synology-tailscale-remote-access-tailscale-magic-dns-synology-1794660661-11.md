---
title: "Fix Tailscale MagicDNS Name Not Resolving for Your Synology NAS"
description: "Troubleshoot why your Synology NAS Tailscale MagicDNS name fails to resolve on a remote client, with step-by-step checks and a verification test."
seo_title: "Tailscale MagicDNS Name Not Resolving on Synology: Fix It"
seo_description: "Troubleshoot why your Synology NAS Tailscale MagicDNS name fails to resolve on a remote client, with step-by-step checks and a verification test."
keywords:
  - "tailscale magic dns synology"
date: 2026-11-17T10:08:16+08:00
publishDate: 2026-11-17T10:08:16+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're connected to Tailscale, but when you try to reach your Synology NAS by its MagicDNS name, the remote client says it cannot resolve the host. This is a common hiccup with **tailscale magic dns synology** setups, and it's usually fixable with a few targeted checks.

## Confirm MagicDNS is enabled and the name is correct

On the NAS, open the Tailscale admin console and go to DNS. Verify MagicDNS is enabled. Then check the Machines list and note the exact MagicDNS name for your NAS—it often looks like `nas-name.tailnet-name.ts.net`. On the remote client, run `tailscale status` and confirm the NAS appears with that same name. If the name differs, you're using the wrong hostname.

## Check DNS resolution on the remote client

Run `nslookup nas-name.tailnet-name.ts.net` (or `dig`) on the remote client. If it returns the NAS's Tailscale IP (100.x.x.x), DNS is working and the problem is elsewhere. If it times out or returns NXDOMAIN, MagicDNS is not being applied. Next, run `tailscale dns status` on the client to see whether MagicDNS is active and which resolvers are in use. If it shows MagicDNS disabled, re-enable it in the admin console and restart the Tailscale client.

## Verify with a direct IP test

Ping the NAS by its Tailscale IP: `ping 100.x.x.x`. If that succeeds but the name fails, the issue is purely DNS. If both fail, fix connectivity first.

## Rollback and recovery

If changes to DNS settings break other name resolution, revert the last change in the admin console and restart Tailscale on both devices. Keep a note of the original MagicDNS name before editing.

Still stuck?

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your `tailscale dns status` output.
