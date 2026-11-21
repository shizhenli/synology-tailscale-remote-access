---
title: "Synology Firewall Blocking Tailscale: How to Confirm and Fix the Drop"
description: "Tailscale shows connected but the Synology firewall drops your remote connection. Follow these checks to confirm the block and restore access safely."
seo_title: "Synology Firewall Tailscale: Fix Blocked Remote Connection"
seo_description: "Tailscale shows connected but the Synology firewall drops your remote connection. Follow these checks to confirm the block and restore access safely."
keywords:
  - "synology firewall tailscale"
date: 2026-11-21T16:17:29+08:00
publishDate: 2026-11-21T16:17:29+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're connected to Tailscale, but the moment you try to reach the NAS, the Synology firewall silently drops the traffic.

## Confirm the firewall is the cause

First, isolate the firewall. Temporarily disable the Synology firewall (Control Panel > Security > Firewall) and retry your remote connection. If access works with the firewall off, you've confirmed the firewall is dropping Tailscale traffic. Re-enable it immediately after this test.

Next, check the firewall rules. Tailscale uses the tailscale0 interface and addresses in the 100.64.0.0/10 range. Open Control Panel > Security > Firewall > Edit Rules and verify there is an allow rule for the tailscale0 interface, or for source 100.64.0.0/10, covering the services you need (for example, SMB on 445 or DSM on 5001). If the only rules are for the LAN interface, Tailscale packets will hit the default deny.

## Interpret the results

If the rule exists but traffic still drops, check rule order: a deny rule above your allow rule wins. Move the Tailscale allow rule above any broad deny. Also confirm the rule is enabled and applies to the correct interface. If no tailscale0 interface appears, Tailscale isn't fully initialized, so restart the Tailscale package before editing rules.

## Verification test

With the corrected rule in place, reconnect from your remote device and open a share or DSM. A successful login confirms the fix. Then run a quick ping to the NAS Tailscale IP (100.x.x.x) to confirm the path is stable.

## Rollback note

If the change makes things worse, disable the firewall again to regain access, then revert your last rule edit. Keep a known-good rule set exported before making changes.

If the **synology firewall tailscale** rule still drops traffic after these checks,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your rule order and interface list.
