---
title: "Synology Tailscale ACL Blocked: Why the Firewall Drops Your Remote Connection"
description: "Tailscale is connected but the Synology firewall drops your remote session. Follow these steps to diagnose and restore access when a synology tailscale acl blocked rule is interfering."
seo_title: "Synology Tailscale ACL Blocked: Fix Firewall Dropping Remote Access"
seo_description: "Tailscale is connected but the Synology firewall drops your remote session. Follow these steps to diagnose and restore access when a synology tailscale acl blocked rule is interfering."
keywords:
  - "synology tailscale acl blocked"
date: 2026-11-28T22:23:42+08:00
publishDate: 2026-11-28T22:23:42+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You are remotely connected to your Synology NAS through Tailscale, but the Synology firewall silently drops the connection, leaving you unable to reach shares or DSM.

## Check the Firewall Rule Order

Open Control Panel > Security > Firewall. Select the profile that applies to your Tailscale interface (usually the default profile). Look at the rule list from top to bottom. Firewall rules are evaluated in order, and the first matching rule wins. If a deny rule for the Tailscale subnet (100.64.0.0/10) appears above your allow rule, traffic is dropped before the allow is ever reached. Move the allow rule above any deny rule that covers the same source range, then click Apply.

## Confirm the Interface and Source

In Tailscale admin, note the NAS's Tailscale IP (100.x.y.z). In the firewall rule, verify the source is either that exact IP or the 100.64.0.0/10 range, and the destination is the NAS. A common mistake is allowing only the LAN subnet (192.168.x.0/24) while remote clients arrive from the Tailscale range. If the rule does not match the actual source, the default deny action applies.

## Verification Test

From the remote device, run `tailscale ping <nas-tailscale-ip>` to confirm the tunnel is healthy, then attempt to open DSM at `http://<nas-tailscale-ip>:5000`. If DSM loads, the firewall now permits the traffic. If it still times out, temporarily disable the firewall for 30 seconds and retry. A successful connection with the firewall off confirms the rule order or source is still wrong.

## Rollback Note

If you changed rule order and lost LAN access, re-enable the previous rule set by restoring your saved firewall configuration from Control Panel > Security > Firewall > Restore. Keep a backup before editing.

If a **synology tailscale acl blocked** condition persists after these checks,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your rule list and Tailscale IPs.
