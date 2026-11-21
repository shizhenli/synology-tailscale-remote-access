---
title: "Tailscale MagicDNS Name Not Resolving for Synology NAS: A Step-by-Step Fix"
description: "Troubleshoot Tailscale MagicDNS resolution failure for your Synology NAS with practical checks, exact result interpretation, and a verification test."
seo_title: "Fix Tailscale MagicDNS Name Not Resolving on Synology NAS"
seo_description: "Troubleshoot Tailscale MagicDNS resolution failure for your Synology NAS with practical checks, exact result interpretation, and a verification test."
keywords:
  - "tailscale magic dns synology"
date: 2026-11-21T16:17:29+08:00
publishDate: 2026-11-21T16:17:29+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected to your Synology NAS via Tailscale, but the MagicDNS name fails to resolve on your client, leaving you stuck before you can diagnose further.

## Check MagicDNS is enabled on your tailnet

Open the Tailscale admin console and go to DNS. Confirm MagicDNS is toggled on. If it's off, enable it and wait a minute. Then on your remote client, run `tailscale status` and look for the NAS name. If it appears with an IP but no hostname resolution, proceed.

## Verify the NAS Tailscale hostname

On the NAS, run `tailscale status --json | grep -i dnsname`. You should see a DNS name like `nas-name.tailnet-name.ts.net`. If it's empty or missing, the NAS hasn't registered its name. Restart Tailscale on the NAS: `sudo synopkg restart Tailscale` (or use Package Center). Then recheck.

## Test resolution from the client

On the remote client, run `nslookup nas-name.tailnet-name.ts.net`. A successful response returns the NAS Tailscale IP (100.x.y.z). If it times out or returns NXDOMAIN, the client isn't using MagicDNS. Check the client's Tailscale DNS settings: `tailscale dns status`. Ensure it shows the tailnet DNS server. If not, run `tailscale up --accept-dns=true`.

## Confirm the expected outcome

After these steps, the magicdns name does not resolve should be resolved: the name now resolves to the NAS IP. Test with `ping nas-name.tailnet-name.ts.net`—you should see replies from the 100.x address.

## Rollback and recovery

If changes break connectivity, revert by disabling MagicDNS in the admin console and restarting Tailscale on the NAS and client. This restores previous IP-based access. For persistent issues,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).

Remember: **tailscale magic dns synology** resolution depends on consistent naming and DNS acceptance on both ends.
