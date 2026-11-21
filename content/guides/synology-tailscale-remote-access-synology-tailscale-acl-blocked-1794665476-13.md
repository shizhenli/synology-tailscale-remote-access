---
title: "Synology Tailscale ACL Blocked: When the Firewall Drops Your Remote Connection"
description: "Troubleshoot Synology Tailscale ACL blocked errors: check firewall rules, verify Tailscale interface, and restore remote access with a controlled diagnosis."
seo_title: "Fix Synology Tailscale ACL Blocked: Firewall Drops Remote Connection"
seo_description: "Troubleshoot Synology Tailscale ACL blocked errors: check firewall rules, verify Tailscale interface, and restore remote access with a controlled diagnosis."
keywords:
  - "synology tailscale acl blocked"
date: 2026-11-21T16:17:29+08:00
publishDate: 2026-11-21T16:17:29+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected to your Synology NAS through Tailscale, but the Synology firewall is dropping the connection, leaving you stuck before you can complete a controlled diagnosis. This article walks you through the exact checks to identify and resolve that firewall block.

## Check the Tailscale Interface and Firewall Rules

First, confirm Tailscale is actually up on the NAS. Open Package Center or SSH and run `tailscale status`. You should see your NAS listed with an IP address in the 100.x.x.x range. If it's not there, Tailscale isn't running and the firewall isn't your issue.

Next, open Control Panel > Security > Firewall. Select the active firewall profile and click Edit Rules. Look for a rule that allows traffic from the Tailscale interface (usually named `tailscale0`). If no such rule exists, the default deny policy will drop all Tailscale traffic. Add a rule to allow all incoming traffic on `tailscale0`, or specifically allow the ports you need (e.g., 5000 for DSM, 445 for SMB).

Also check the rule order: a deny-all rule above your Tailscale allow rule will block it. Drag the allow rule above any blanket deny.

## Verify the Fix

From your remote device, ping the NAS Tailscale IP (e.g., `ping 100.x.x.x`). If replies come back, the firewall is no longer dropping traffic. Then try accessing DSM via `http://100.x.x.x:5000`. If it loads, the block is resolved.

## Rollback Note

If you added a broad allow rule and want to revert, simply disable or delete that rule in the firewall list. The previous deny policy will resume, so test access again afterward.

If the firewall rules look correct but connections still fail, the issue may be a **synology tailscale acl blocked** entry in Tailscale's admin console. Review your ACL policy to ensure your user or device is permitted to reach the NAS. For guided assistance,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
