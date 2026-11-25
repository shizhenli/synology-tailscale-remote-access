---
title: "Mobile Data Fails with Tailscale Exit Node on Synology: Diagnose the Path"
description: "Troubleshoot Synology Tailscale mobile data and exit node failures with step-by-step checks, exact result interpretation, and a verification test."
seo_title: "Synology Tailscale Mobile Data: Exit Node Fails? Fix It"
seo_description: "Troubleshoot Synology Tailscale mobile data and exit node failures with step-by-step checks, exact result interpretation, and a verification test."
keywords:
  - "synology tailscale mobile data"
date: 2026-11-25T10:08:26+08:00
publishDate: 2026-11-25T10:08:26+08:00
draft: false
categories:
  - "Remote Access"
category_slug: "remote-access"
---

You're trying to reach your Synology NAS over Tailscale, but when your phone switches to mobile data or you enable an exit node, the connection drops before you can complete a controlled diagnosis.

## Confirm the failure point

Start by isolating whether the problem is mobile data or the exit node. On your phone, open the Tailscale app and note the connection status. If it says connected but the NAS is unreachable, the tunnel is up but routing is wrong. Next, disable the exit node and retry over mobile data. If the NAS becomes reachable, the exit node is the culprit. If it stays unreachable, mobile data is the issue.

## Check exit node routing

On the Synology, open Tailscale and verify the exit node is advertised and approved. On the phone, confirm the exit node is selected. Then test with a simple ping to the NAS Tailscale IP. A timeout means traffic is not being routed back; a reply means the path works and the problem is elsewhere, such as DNS or a share-level permission. This is the most common **synology tailscale mobile data** failure: the exit node sends traffic out but not back to the NAS.

## Verify with a controlled test

Switch the phone to Wi-Fi, disable the exit node, and confirm the NAS is reachable. Then enable the exit node on Wi-Fi. If it fails only on mobile data, check your carrier's APN settings for IPv6 or private DNS that may block Tailscale. If it fails on both, re-approve the exit node on the Synology and restart the Tailscale service.

## Rollback and recovery

If changes make things worse, disable the exit node and revert any DNS or APN edits. Restart Tailscale on both devices. For guided recovery,

![A phone on cellular data opening a Synology share through Tailscale](/images/service-detail-960.webp "Synology Tailscale cellular access test")

[Ask for help](/#consult).
