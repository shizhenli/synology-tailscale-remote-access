---
title: "Synology Tailscale Not Connecting: Fix 'Connected but NAS Share Unreachable'"
description: "Tailscale shows connected but your Synology NAS share is unreachable? Follow these practical checks to diagnose and restore SMB or DSM access."
seo_title: "Synology Tailscale Not Connecting: Fix 'Connected but NAS Share Unreachable'"
seo_description: "Tailscale shows connected but your Synology NAS share is unreachable? Follow these practical checks to diagnose and restore SMB or DSM access."
keywords:
  - "synology tailscale not connecting"
date: 2026-11-21T16:14:26+08:00
publishDate: 2026-11-21T16:14:26+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remote, Tailscale says connected, but the NAS share or DSM won't open, and you're stuck before any controlled diagnosis. Let's fix that.

## Check the Tailscale IP and ping
On the remote device, open Tailscale and note the NAS's Tailscale IP (100.x.x.x). Ping it. If ping fails, Tailscale is connected but the NAS isn't responding on that IP—likely the Tailscale package is stopped or the NAS is asleep. If ping succeeds, move on.

## Verify SMB/DSM ports and services
From the remote device, test the actual service. For SMB, run `nc -vz <tailscale-ip> 445` (or use a port checker). For DSM, try `https://<tailscale-ip>:5001`. If the port is closed, the service isn't listening on the Tailscale interface. On the NAS, open Control Panel > File Services and confirm SMB is enabled; for DSM, confirm Web Station or the DSM port is correct. Also check Control Panel > Security > Firewall: ensure the Tailscale interface (or 100.64.0.0/10) is allowed, not just the LAN.

## Confirm Tailscale ACLs and sharing
In the Tailscale admin console, verify the remote device and NAS are in the same tailnet and that ACLs permit traffic on ports 445 and 5001. A common cause of **synology tailscale not connecting** to shares is an ACL that allows ping but blocks SMB. If you use Tailscale SSH or subnet routes, confirm they aren't conflicting.

## Verification test
After changes, restart the Tailscale package on the NAS (Package Center > Tailscale > Stop, then Start). From the remote device, ping the Tailscale IP, then open `\\<tailscale-ip>\share` (Windows) or `smb://<tailscale-ip>/share` (macOS). DSM should load at `https://<tailscale-ip>:5001`. Success means the share opens without LAN access.

## Rollback note
If access breaks after firewall or ACL edits, revert the last change: disable the new firewall rule or restore the previous ACL, then restart Tailscale. Keep a note of the original working rule so you can restore it quickly.

Still stuck after these checks?

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
