---
title: "Synology Firewall Tailscale: When the Firewall Drops Your Remote Connection"
description: "Tailscale is up but the Synology firewall drops the remote connection. Follow these checks to confirm the block and restore access."
seo_title: "Fix Synology Firewall Blocking Tailscale Traffic"
seo_description: "Tailscale is up but the Synology firewall drops the remote connection. Follow these checks to confirm the block and restore access."
keywords:
  - "synology firewall tailscale"
date: 2026-11-24T12:10:51+08:00
publishDate: 2026-11-24T12:10:51+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're connected to Tailscale, but the Synology firewall is silently dropping your remote session, and you need to confirm and fix that block.

## Confirm the firewall is the cause

Open DSM, go to Control Panel > Security > Firewall, and check whether the active profile has a rule that denies Tailscale's interface. Tailscale on Synology typically uses the tun interface (often tun0) and the 100.64.0.0/10 address range. If your profile allows only the LAN subnet, remote Tailscale clients fall outside it and get dropped. A quick result check: temporarily disable the firewall profile and retry the connection. If access returns, the firewall is the blocker. Re-enable it immediately after this test.

## Add a targeted allow rule

Create a rule above any deny rules that allows the Tailscale interface or the 100.64.0.0/10 source range to reach the services you need, such as DSM (5000/5001) or SMB (445). Keep the rule narrow; do not open all ports. Save and apply the profile.

## Verification test

From the remote Tailscale device, ping the NAS Tailscale IP, then open DSM or mount the share. If both succeed with the firewall enabled, the rule is working. If ping works but services fail, the rule is too narrow for those ports.

## Rollback note

If the new rule causes unexpected local lockouts, disable the firewall profile, remove the rule, and re-enable the previous profile. Keep console or local access available before editing firewall rules.

If the **synology firewall tailscale** block persists after these checks, the rule order or interface name may differ on your model.

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your firewall profile and Tailscale interface details.
