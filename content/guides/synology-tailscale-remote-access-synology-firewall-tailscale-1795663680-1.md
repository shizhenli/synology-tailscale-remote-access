---
title: "Synology Firewall Tailscale: When the Firewall Blocks Tailscale Traffic"
description: "Tailscale is up but the Synology firewall drops the remote connection. Follow these checks to confirm the block, test access, and restore remote connectivity safely."
seo_title: "Synology Firewall Tailscale: Fix Blocked Remote Access"
seo_description: "Tailscale is up but the Synology firewall drops the remote connection. Follow these checks to confirm the block, test access, and restore remote connectivity safely."
keywords:
  - "synology firewall tailscale"
date: 2026-11-28T22:23:42+08:00
publishDate: 2026-11-28T22:23:42+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're connected to Tailscale, but the Synology firewall is dropping your remote connection, and you want to confirm and fix the block before changing anything else.

## Confirm the firewall is the cause

First, verify Tailscale itself is healthy. In the Synology Tailscale package, check that the status shows connected and note the NAS Tailscale IP (100.x.x.x). From your remote device, run `tailscale ping <NAS-Tailscale-IP>`. If that succeeds but a normal ping or SMB/DSM connection fails, the tunnel is fine and the firewall is the likely blocker.

Next, open Control Panel > Security > Firewall. Confirm the firewall is enabled and review the active profile. Look for rules that allow Tailscale traffic. The Tailscale interface typically appears as `tailscale0`; if no rule permits traffic on that interface or from the 100.64.0.0/10 range, your **synology firewall tailscale** traffic will be dropped. Check the rule order too: a deny-all rule above your allow rule will still block the connection.

## Apply a minimal fix and test

Add an allow rule for the Tailscale interface (or the 100.64.0.0/10 source range) for the services you need, such as DSM 5001 or SMB 445, and place it above any deny-all rule. Save and apply.

Verification test: from the remote device, reconnect to the NAS Tailscale IP on the service port. If it opens, the firewall was the cause. If it still fails, temporarily disable the firewall, test again, then re-enable it immediately. A successful connection with the firewall off confirms a rule problem rather than a Tailscale problem.

Rollback note: before editing, export or screenshot your firewall rules. If the new rule causes unexpected blocking, disable the firewall, restore the previous rules, and re-enable it.

If the rule looks correct but traffic is still dropped,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your firewall profile and rule order.
