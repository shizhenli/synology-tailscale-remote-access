---
title: "Synology Firewall Tailscale: When the Firewall Drops the Remote Connection"
description: "Tailscale is up but the Synology firewall drops the remote connection. Follow these checks to diagnose and restore access, with a safe rollback step."
seo_title: "Synology Firewall Tailscale: Fix Dropped Remote Connection"
seo_description: "Tailscale is up but the Synology firewall drops the remote connection. Follow these checks to diagnose and restore access, with a safe rollback step."
keywords:
  - "synology firewall tailscale"
date: 2026-11-23T08:08:30+08:00
publishDate: 2026-11-23T08:08:30+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're connected to Tailscale, but the Synology firewall is silently dropping your remote session, and you want to confirm and fix that block without guessing.

## Confirm the Firewall Is the Cause

First, verify the drop is firewall-related, not a Tailscale failure. From your remote device, ping the NAS Tailscale IP (100.x.x.x). If ping fails while the Tailscale admin console shows the NAS online, the firewall is likely dropping traffic. Next, log into DSM and open Control Panel > Security > Firewall. Check whether the active profile has a rule that denies the Tailscale interface (usually tailscale0) or the 100.64.0.0/10 range. If you see a deny rule above any allow rule, that is your block.

## Adjust the Rule Order

Firewall rules are evaluated top-down. Create an allow rule for the Tailscale interface or the 100.64.0.0/10 subnet and drag it above the deny rule. Apply the change, then test again. If the connection still drops, temporarily disable the firewall for 60 seconds and retry. If access returns with the firewall off, the rule order or interface binding is still wrong.

## Verification Test

From the remote device, run a ping to the NAS Tailscale IP and then open a DSM share. Both should succeed. Also check Control Panel > Security > Firewall > Log to confirm no new deny entries appear for the Tailscale interface during the test.

## Rollback Note

Before editing rules, export the current firewall configuration. If remote access breaks completely, re-enable the previous profile or restore the export from local access. Do not leave the firewall disabled permanently.

If the **synology firewall tailscale** rule still drops traffic after these checks,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your firewall log excerpt.
