---
title: "Synology Firewall Drops Tailscale Traffic: A Step-by-Step Diagnosis"
description: "Tailscale is up but the Synology firewall drops remote connections? Follow these checks to diagnose and restore access, with a safe rollback."
seo_title: "Synology Firewall Tailscale: Fix Dropped Remote Connections"
seo_description: "Tailscale is up but the Synology firewall drops remote connections? Follow these checks to diagnose and restore access, with a safe rollback."
keywords:
  - "synology firewall tailscale"
date: 2026-11-21T16:11:24+08:00
publishDate: 2026-11-21T16:11:24+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

## Understand the symptom

You are connected to Tailscale on your Synology NAS, but remote access fails because the firewall is dropping the traffic. This is a common **synology firewall tailscale** issue, and it usually comes down to the firewall not allowing the Tailscale interface or the return traffic.

## Check the firewall rules

Open Control Panel > Security > Firewall. Select the profile that is active (usually Default) and click Edit Rules. Look for a rule that allows all traffic on the Tailscale interface (often named tailscale0). If no such rule exists, create one: Allow, all ports, all protocols, source interface tailscale0. Also check that the default policy is not set to Deny All without exceptions. If you see a rule blocking the Tailscale subnet (100.64.0.0/10), remove or disable it.

Interpretation: if the Tailscale interface rule is missing or the default policy denies without exceptions, the firewall will drop remote connections even though Tailscale is up.

## Verify the fix

From a remote device on your tailnet, ping the NAS Tailscale IP (100.x.x.x). Then try to open a shared folder or the DSM login page via that IP. If both succeed, the firewall is no longer dropping Tailscale traffic. If ping works but services fail, check service-specific firewall rules.

## Rollback if needed

If you lose local access after changing rules, connect to the NAS locally and disable the firewall temporarily (Control Panel > Security > Firewall > Disable). Then re-enable and adjust the rule order so the Tailscale allow rule sits above any deny rules. For guided help,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
