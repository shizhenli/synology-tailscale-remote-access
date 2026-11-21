---
title: "Tailscale DNS Not Resolving: MagicDNS Name Fails for Synology NAS"
description: "Troubleshoot Tailscale DNS not resolving on your Synology NAS: check MagicDNS, DNS settings, and client configuration to restore name resolution."
seo_title: "Fix Tailscale DNS Not Resolving on Synology NAS (MagicDNS)"
seo_description: "Troubleshoot Tailscale DNS not resolving on your Synology NAS: check MagicDNS, DNS settings, and client configuration to restore name resolution."
keywords:
  - "tailscale dns not resolving"
date: 2026-11-21T16:11:24+08:00
publishDate: 2026-11-21T16:11:24+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You are remotely connected to your Synology NAS via Tailscale, but the MagicDNS name fails to resolve on your client, leaving you stuck before you can diagnose further.

## Check MagicDNS on the Tailscale Admin Console

Open the Tailscale admin console and go to the DNS tab. Verify that MagicDNS is enabled. If it is off, turn it on and save. Next, confirm that the NAS appears in the machine list with its Tailscale IP and that no DNS conflicts exist (such as a custom nameserver overriding MagicDNS). If MagicDNS is already on, proceed to the client.

## Verify Client DNS Settings

On the remote client, check whether it is using Tailscale's DNS. For Windows, run `tailscale status` and look for the DNS line; it should show the Tailscale DNS server (usually 100.100.100.100). On macOS or Linux, run `tailscale dns status` or check `/etc/resolv.conf` for the same address. If the client uses a different DNS server, MagicDNS queries will fail. This is a common cause of **tailscale dns not resolving** when the NAS name is correct but the client ignores Tailscale's resolver.

## Test Resolution Directly

From the client, run `nslookup <nas-name>` or `ping <nas-name>`. If it returns the NAS Tailscale IP (100.x.x.x), resolution works. If it fails, try `nslookup <nas-name> 100.100.100.100` to query MagicDNS directly. A successful response confirms the client's default resolver is the problem; a failure points to the NAS or tailnet configuration.

## Rollback and Recovery

If changes to DNS settings break connectivity, revert to the previous configuration in the admin console and restart the Tailscale service on the NAS (`sudo tailscale down && sudo tailscale up`). This restores the prior state without losing tailnet membership.

If the name still does not resolve after these checks,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) for a guided diagnosis.
