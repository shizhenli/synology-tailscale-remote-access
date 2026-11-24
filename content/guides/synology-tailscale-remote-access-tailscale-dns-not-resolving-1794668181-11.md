---
title: "Fix Tailscale MagicDNS Name Not Resolving on Synology NAS: A Step-by-Step Diagnosis"
description: "Troubleshoot why your Synology NAS Tailscale MagicDNS name fails to resolve on a remote client. Step-by-step checks, verification, and rollback."
seo_title: "Tailscale DNS Not Resolving on Synology: Fix MagicDNS Name"
seo_description: "Troubleshoot why your Synology NAS Tailscale MagicDNS name fails to resolve on a remote client. Step-by-step checks, verification, and rollback."
keywords:
  - "tailscale dns not resolving"
date: 2026-11-24T11:08:41+08:00
publishDate: 2026-11-24T11:08:41+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're trying to reach your Synology NAS remotely through Tailscale, but when you use its MagicDNS name, the remote client says the host cannot be found.

This is a common **tailscale dns not resolving** symptom, and it usually comes down to one of three things: MagicDNS is disabled in your tailnet, the client isn't using Tailscale's DNS, or the NAS's Tailscale node hasn't advertised its name properly. Work through these checks in order.

## Check MagicDNS in the admin console

Open the Tailscale admin console and go to DNS. Confirm MagicDNS is enabled. If it's off, turn it on and save. Then, on the remote client, run `tailscale status` and look for the NAS name in the list. If the name appears but still won't resolve, move to the next check.

## Verify the client's DNS settings

On the remote device, run `tailscale dns status` (or check the Tailscale app's DNS tab). You should see the MagicDNS resolver listed as active. If it's missing, toggle Tailscale off and on, or reinstall the client. On Synology, ensure the Tailscale package is running and the NAS shows as connected in the admin console.

## Confirm the NAS node name

In the admin console, find your NAS under Machines. Note its exact MagicDNS name (often `nas-name.tailnet-name.ts.net`). Try pinging that full name from the remote client. If the short name fails but the full name works, your client's search domain isn't set—use the full name as a workaround.

## Verification test

From the remote client, run `ping nas-name.tailnet-name.ts.net`. A successful reply confirms resolution is restored. Then try accessing a shared folder via the MagicDNS name.

## Rollback note

If changes break connectivity, revert the DNS settings in the admin console and restart the Tailscale service on both devices. If the issue persists,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
