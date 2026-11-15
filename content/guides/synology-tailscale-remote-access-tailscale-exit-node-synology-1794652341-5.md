---
title: "Tailscale Exit Node on Synology: When Mobile Data or Exit Node Fails"
description: "Troubleshoot Tailscale exit node or mobile-data failures on Synology NAS with step-by-step checks, verification, and rollback guidance."
seo_title: "Fix Tailscale Exit Node on Synology: Mobile Data and Exit Node Failures"
seo_description: "Troubleshoot Tailscale exit node or mobile-data failures on Synology NAS with step-by-step checks, verification, and rollback guidance."
keywords:
  - "tailscale exit node synology"
date: 2026-11-15T19:05:58+08:00
publishDate: 2026-11-15T19:05:58+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You are trying to reach your Synology NAS remotely through Tailscale, but the connection fails specifically when your phone switches to mobile data or when you route traffic through an exit node.

## Check the exit node advertisement and approval

On the Synology, open Tailscale and confirm the exit node option is enabled. In the Tailscale admin console, go to Machines, select the Synology, and verify the exit node is approved. If it shows as unapproved, approve it and wait for the status to update. An unapproved exit node will silently drop traffic, which matches your symptom.

## Test the mobile-data path without the exit node

On your phone, disconnect Wi-Fi, keep mobile data on, and open Tailscale. Disable the exit node selection, then try to reach the NAS by its Tailscale IP. If this works, the exit node is the failing component. If it also fails, the mobile-data path itself is blocked, often by carrier restrictions or a local firewall rule on the Synology.

## Verify with a controlled test

Re-enable the exit node on the phone. Run a quick request to the NAS Tailscale IP and note whether it times out or returns data. Then switch the exit node off and repeat. A consistent failure only with the exit node active confirms the **tailscale exit node synology** configuration needs attention. Check that the Synology firewall allows forwarded traffic and that no rule blocks the Tailscale interface.

## Rollback and recovery

If changes make access worse, disable the exit node on the phone and revert any recent firewall rule on the Synology. Confirm basic NAS access over Tailscale before re-enabling the exit node.

If the failure persists after these checks,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your Tailscale status output and firewall rules.
