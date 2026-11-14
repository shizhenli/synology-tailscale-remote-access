---
title: "Tailscale DNS Not Resolving: Fix the MagicDNS Name for Your Synology NAS"
description: "Troubleshoot Tailscale MagicDNS not resolving your Synology NAS name on a remote client. Step-by-step checks, verification, and rollback for a controlled fix."
seo_title: "Tailscale DNS Not Resolving on Synology: MagicDNS Fix"
seo_description: "Troubleshoot Tailscale MagicDNS not resolving your Synology NAS name on a remote client. Step-by-step checks, verification, and rollback for a controlled fix."
keywords:
  - "tailscale dns not resolving"
date: 2026-11-14T19:47:13+08:00
publishDate: 2026-11-14T19:47:13+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're away from home, Tailscale shows connected, but typing your NAS's MagicDNS name on your laptop returns "server not found." This is a classic **tailscale dns not resolving** symptom, and it's usually fixable in a few minutes.

## Check MagicDNS is enabled on the tailnet

Open the Tailscale admin console and go to DNS. Confirm MagicDNS is on. If it's off, turn it on and save. On the remote client, run `tailscale status` and look for the NAS's MagicDNS name (usually `nasname.tailnet-name.ts.net`). If the name appears, DNS is being advertised; if not, the client may be using an old profile.

## Verify the client's DNS settings

On the remote client, run `tailscale dns status` (or check the Tailscale tray menu under DNS). You should see the tailnet DNS suffix and the NAS name listed. If the suffix is missing, toggle Tailscale off and on to reapply DNS settings. On Synology, open Tailscale in Package Center and confirm it's running; restart the package if the NAS name is missing from `tailscale status`.

## Test resolution directly

From the remote client, run `nslookup nasname.tailnet-name.ts.net` and then `ping nasname.tailnet-name.ts.net`. A successful nslookup returns the NAS's 100.x address; ping should reply. If nslookup fails but ping by 100.x IP works, DNS is the issue, not connectivity.

## Rollback and recovery

If changes make things worse, disable MagicDNS, save, then re-enable it. On the client, run `tailscale down` then `tailscale up` to reset DNS. If the NAS name still won't resolve, remove and re-add the NAS device in the admin console, then reauthenticate the Synology package.

If these steps don't restore resolution,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your `tailscale status` and `nslookup` output for a controlled diagnosis.
