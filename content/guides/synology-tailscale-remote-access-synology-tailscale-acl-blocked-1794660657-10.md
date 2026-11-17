---
title: "Synology Tailscale ACL Blocked: When the Firewall Drops Remote Traffic"
description: "Tailscale shows connected but the Synology firewall drops your remote session. Walk through targeted checks to confirm the block, verify a fix, and roll back safely."
seo_title: "Synology Tailscale ACL Blocked: Firewall Drops Remote Connection Fix"
seo_description: "Tailscale shows connected but the Synology firewall drops your remote session. Walk through targeted checks to confirm the block, verify a fix, and roll back safely."
keywords:
  - "synology tailscale acl blocked"
date: 2026-11-17T09:28:03+08:00
publishDate: 2026-11-17T09:28:03+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You are connected to Tailscale on your phone, the NAS shows online, yet every remote request dies at the firewall before you can finish a controlled diagnosis.

## Confirm the block is at the firewall layer

Open DSM, then Control Panel > Security > Firewall. Select the profile currently applied to the Tailscale interface (usually the default profile unless you created a dedicated one). Check whether the rule order lets Tailscale traffic reach DSM before any deny rule. A common mistake is a broad deny-all rule sitting above the allow rule for the Tailscale subnet 100.64.0.0/10 or for the specific tailscale0 interface.

Next, open Package Center and confirm Tailscale is running, then check Control Panel > Network > Network Interface for tailscale0. If the interface is up but no allow rule references it, the firewall is dropping traffic even though Tailscale reports connected. That is your **synology tailscale acl blocked** condition in practice.

## Apply a minimal, testable rule

Create an allow rule above the deny rules: source 100.64.0.0/10, destination your NAS, service DSM (5000/5001) plus any share ports you need. Save and apply the profile. Do not disable the firewall entirely; keep the change narrow so you can attribute the result.

## Verification test

From the remote device, disconnect and reconnect Tailscale, then load DSM at the NAS Tailscale IP. If the login page appears within a few seconds, the firewall path is open. If it still times out, recheck rule order and interface binding.

## Rollback note

If the new rule breaks local access, edit the profile and move the allow rule back below your existing LAN rules, or delete it and reapply. Keep a note of the original order before saving.

If rule order and interface binding both look correct and traffic still drops,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) with your profile export.
