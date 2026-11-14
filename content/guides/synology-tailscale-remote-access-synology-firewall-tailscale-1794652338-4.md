---
title: "Synology Firewall Blocking Tailscale: Diagnose and Restore Remote Access"
description: "Tailscale shows connected but Synology firewall drops remote access? Follow these checks to diagnose and restore the expected firewall behavior."
seo_title: "Synology Firewall Tailscale: Fix Blocked Remote Connection"
seo_description: "Tailscale shows connected but Synology firewall drops remote access? Follow these checks to diagnose and restore the expected firewall behavior."
keywords:
  - "synology firewall tailscale"
date: 2026-11-14T19:47:13+08:00
publishDate: 2026-11-14T19:47:13+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

**synology firewall tailscale**. You're connected to Tailscale, but the Synology firewall is silently dropping your remote session, leaving you stuck before you can even run a proper diagnosis.

## Confirm the firewall is the cause

First, isolate the firewall. Temporarily disable the Synology firewall (Control Panel > Security > Firewall) and retry your remote connection. If access returns immediately, the firewall is confirmed as the blocker. Re-enable it right away before continuing.

## Check the Tailscale interface rule

The firewall must allow traffic on the Tailscale interface (usually tailscale0). In Control Panel > Security > Firewall > Edit Rules, verify there is an allow rule for the tailscale0 interface, or that your custom rules don't inadvertently deny it. A common mistake is a deny-all rule placed above the Tailscale allow rule; order matters. Move the allow rule above any broad deny.

## Verify the source and port

If you use subnet routing or specific services, confirm the rule permits the Tailscale IP range (100.64.0.0/10) and the required ports. For DSM access, port 5000/5001 must be allowed from the Tailscale interface. Check that the rule's source is set to the Tailscale interface, not "All," which can conflict with other deny rules.

## Verification test

With the corrected rule in place, reconnect via Tailscale. Then, from another Tailscale device, ping the NAS Tailscale IP and attempt to open DSM. A successful ping and login confirm the firewall now passes traffic. If it still fails, re-check rule order and interface selection.

## Rollback note

If changes worsen access, re-enable the previous firewall configuration or temporarily disable the firewall to regain local access, then review logs under Log Center for denied packets.

If you're still stuck after these steps,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) and we'll review your specific rule set.
