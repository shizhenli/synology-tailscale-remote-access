---
title: "Synology Firewall Tailscale: Restore Remote Access When the Firewall Drops Tailscale Traffic"
description: "Tailscale shows connected but the Synology firewall drops remote access. Diagnose synology firewall tailscale rules and restore connectivity with a controlled test."
seo_title: "Synology Firewall Tailscale: Fix Dropped Remote Connections"
seo_description: "Tailscale shows connected but the Synology firewall drops remote access. Diagnose synology firewall tailscale rules and restore connectivity with a controlled test."
keywords:
  - "synology firewall tailscale"
date: 2026-11-16T12:29:44+08:00
publishDate: 2026-11-16T12:29:44+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're connected to Tailscale, but the Synology firewall is silently dropping your remote session before you can finish a controlled diagnosis.

## Confirm the firewall is the culprit

First, open Control Panel > Security > Firewall and check whether the enabled profile applies to the Tailscale interface (usually tailscale0). If the profile is active and no rule explicitly allows that interface, the firewall is dropping packets even though Tailscale reports connected. Next, inspect the rule order: a deny-all rule placed above your Tailscale allow rule will win. The expected result is a specific allow rule for the tailscale0 interface or your Tailscale subnet, positioned above any deny rule.

## Add a minimal allow rule

Create a rule that permits all traffic on the tailscale0 interface, or restrict it to your Tailscale subnet (100.64.0.0/10) and the ports you need (5000/5001 for DSM, 445 for SMB). Save and apply. Do not disable the firewall entirely; a narrow allow rule is safer and easier to reverse.

## Verification test

From the remote device, run `tailscale ping <nas-tailscale-ip>` to confirm the tunnel, then open DSM at `http://<nas-tailscale-ip>:5000`. If DSM loads, the **synology firewall tailscale** rule is working. If ping succeeds but DSM times out, the rule is still blocking the service port.

## Rollback and recovery

If the new rule locks you out, reconnect locally and disable the firewall profile temporarily, then correct the rule order. Keep a local admin session open while editing firewall rules so you can revert immediately.

If the rule looks correct but traffic still drops, the profile may be bound to the wrong interface. For a guided review,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
