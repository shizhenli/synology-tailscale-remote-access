---
title: "Synology Tailscale Shows Connected but SMB/DSM Unreachable: A Step-by-Step Diagnosis"
description: "Tailscale says connected but your Synology NAS share is unreachable? Follow these practical checks to diagnose SMB or DSM access issues and restore remote access."
seo_title: "Synology Tailscale Not Connecting: Fix SMB/DSM Unreachable When Connected"
seo_description: "Tailscale says connected but your Synology NAS share is unreachable? Follow these practical checks to diagnose SMB or DSM access issues and restore remote access."
keywords:
  - "synology tailscale not connecting"
date: 2026-11-18T10:33:34+08:00
publishDate: 2026-11-18T10:33:34+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're remotely connected to your Synology NAS through Tailscale, the status shows connected, but you can't open SMB shares or DSM—this guide walks you through a controlled diagnosis.

## Confirm the Tailscale IP and basic reachability

First, verify the NAS's Tailscale IP address in the Tailscale admin console or on the NAS itself. From your remote device, open a terminal and run `ping <tailscale-ip>`. If ping fails, the Tailscale tunnel isn't passing traffic—check that both devices are logged into the same tailnet and that the NAS's Tailscale service is running. If ping succeeds, proceed.

## Check SMB and DSM services on the NAS

Next, confirm the services are enabled. In DSM, go to Control Panel > File Services > SMB and ensure SMB is enabled. For DSM access, check Control Panel > Terminal & SNMP and verify HTTP/HTTPS services are running. Also, inspect the Synology firewall: Control Panel > Security > Firewall. If enabled, ensure the Tailscale interface (usually `tailscale0`) is allowed, or temporarily disable the firewall to test. A common cause of **synology tailscale not connecting** to shares is a firewall rule blocking the Tailscale subnet.

## Verify from the remote device

On your remote device, try accessing the NAS using its Tailscale IP: for SMB, use `\\<tailscale-ip>` in File Explorer or `smb://<tailscale-ip>` in Finder; for DSM, open `http://<tailscale-ip>:5000`. If these fail but ping works, the issue is likely service binding or firewall. Check that SMB is bound to all interfaces, not just LAN.

## Verification test

After adjustments, run `ping <tailscale-ip>` again, then attempt to mount the share or open DSM. Success means the share opens or DSM login appears.

## Rollback and recovery

If changes break local access, re-enable the firewall and revert any interface binding changes. Keep a note of original settings. For persistent issues,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult) from our team.
