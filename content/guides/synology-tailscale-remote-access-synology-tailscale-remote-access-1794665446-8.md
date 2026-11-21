---
title: "Synology Tailscale Remote Access: Connected but NAS Share Unreachable"
description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow these steps to diagnose and restore SMB access over Tailscale."
seo_title: "Fix Synology Tailscale Remote Access When NAS Share Is Unreachable"
seo_description: "Tailscale shows connected but your Synology NAS share or DSM is unreachable? Follow these steps to diagnose and restore SMB access over Tailscale."
keywords:
  - "synology tailscale remote access"
date: 2026-11-21T16:14:26+08:00
publishDate: 2026-11-21T16:14:26+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're connected to Tailscale, but when you try to open your Synology NAS share or DSM, nothing loads—this guide helps you diagnose that exact "connected but unreachable" state.

## Confirm the Tailscale IP and basic reachability

First, on the remote device, open Tailscale and note the NAS's Tailscale IP (usually 100.x.x.x). Then try to ping that IP. If ping fails, Tailscale is connected but the NAS isn't responding on its tailnet address—check that Tailscale is running on the NAS and that the NAS shows as connected in the Tailscale admin console. If ping succeeds, proceed to the next check.

## Test DSM and SMB separately

Open a browser and go to http://[Tailscale-IP]:5000 (or https://[Tailscale-IP]:5001). If DSM loads but SMB doesn't, the issue is SMB-specific. If neither loads, the problem is broader. For SMB, try connecting to \\[Tailscale-IP]\sharename from File Explorer or Finder. A timeout suggests a firewall or service block; an authentication prompt means SMB is reachable but credentials or permissions need attention.

## Check Synology firewall and Tailscale interface

In DSM, go to Control Panel > Security > Firewall. Ensure the Tailscale interface (tailscale0) is allowed, or that the firewall rule permits the Tailscale subnet (100.64.0.0/10). Also verify that SMB Service is enabled under Control Panel > File Services > SMB. If you recently changed firewall rules, disable the firewall temporarily to test—then re-enable and add an allow rule for Tailscale.

## Verification test

From the remote device, run: ping [Tailscale-IP] (should reply), then open DSM and mount the SMB share. If both work, the issue is resolved. If only ping works, revisit the SMB service and firewall settings.

## Rollback note

If a firewall change breaks local access, revert to the previous rule set via DSM's firewall backup or by temporarily disabling the firewall. Always keep a local admin session open when editing firewall rules.

For a controlled diagnosis of your **synology tailscale remote access** setup,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
