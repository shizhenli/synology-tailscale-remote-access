---
title: "Synology Tailscale ACL Blocked: Fix Firewall Drops on Remote Connections"
description: "Tailscale is up but Synology firewall drops remote connections? Diagnose the synology tailscale acl blocked issue with firewall checks, log interpretation, and a rollback step."
seo_title: "Synology Tailscale ACL Blocked: Firewall Drops Remote Connection Fix"
seo_description: "Tailscale is up but Synology firewall drops remote connections? Diagnose the synology tailscale acl blocked issue with firewall checks, log interpretation, and a rollback step."
keywords:
  - "synology tailscale acl blocked"
date: 2026-11-23T09:08:26+08:00
publishDate: 2026-11-23T09:08:26+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're connected to Tailscale, but when you try to reach your Synology NAS remotely, the connection just dies—likely because the Synology firewall is dropping Tailscale traffic before it ever reaches DSM.

## Check the firewall rule order
Open Control Panel > Security > Firewall. Select the profile that's active (usually the default). Look for a rule allowing the Tailscale interface (tailscale0) or the 100.x.y.z address range. If that rule sits below a deny-all rule, it never gets evaluated. Drag the Tailscale allow rule above any broad deny rule. Apply and test immediately.

## Verify the interface and source
In the same firewall profile, confirm the rule's source is the Tailscale interface, not just an IP. If you used an IP, make sure it matches the NAS's current Tailscale address. A mismatch here is a common cause of a **synology tailscale acl blocked** symptom—the firewall sees the packet but no rule matches, so it drops it.

## Read the logs
Go to Log Center > Firewall. Attempt a remote connection, then refresh. If you see drops on the Tailscale interface with no matching allow, the rule order or source is wrong. If you see no entries at all, the traffic isn't reaching the firewall—check Tailscale status first.

## Verification test
From a remote device on Tailscale, run ping to the NAS's Tailscale IP, then open DSM in a browser. Success means the firewall now passes traffic. If ping works but DSM doesn't, the issue is service-level, not firewall.

## Rollback note
Before editing rules, export the firewall profile. If remote access breaks entirely, re-import the saved profile to restore the previous state.

If the logs still show drops after reordering,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your firewall export and a log snippet.
