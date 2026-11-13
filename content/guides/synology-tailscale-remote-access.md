---
title: 'Synology Tailscale Access: Test from Cellular, Not Home Wi-Fi'
description: A remote-access test that checks Tailscale state, the correct address, Synology permissions, and a real file read over cellular.
seo_title: Synology Tailscale Not Working on Cellular?
seo_description: A remote-access test that checks Tailscale state, the correct address, Synology permissions, and a real file read over cellular.
keywords:
- synology tailscale not working
- synology remote access cellular
- tailscale share access
date: '2026-11-13'
draft: false
categories:
- Remote Access
category_slug: remote-access
---

<p>A Synology share that works on home Wi-Fi can still fail remotely because the client is using a LAN address, Tailscale is offline, or the share permissions are wrong.</p>
<h2>Before you start</h2><ul>
<li>Synology NAS with Tailscale installed</li>
<li>One phone or laptop with Tailscale</li>
<li>A test share and a small readable file</li>
<li>Mobile data available</li>
</ul>
<h2>Step-by-step checks</h2>
<h3>Step 1: Verify both Tailscale nodes</h3>
<p>Open the Tailscale admin/client view and confirm the NAS and remote device are online in the same tailnet. Check key expiry and the last-seen time.</p>
<p><strong>Check:</strong> Both devices show connected and are not key-expired.</p>
<h3>Step 2: Use the Tailscale address</h3>
<p>Find the NAS Tailscale IP or MagicDNS name. Replace the old 192.168.x.x address in the client with the Tailscale address.</p>
<p><strong>Check:</strong> The client is configured with the Tailscale address, not the LAN IP.</p>
<h3>Step 3: Switch to cellular</h3>
<p>Turn off Wi-Fi on the phone, wait for mobile data, and confirm Tailscale remains connected. Open the NAS URL or app from the Tailscale address.</p>
<p><strong>Check:</strong> The client reaches the Synology login over cellular.</p>
<h3>Step 4: Open a named share and file</h3>
<p>Sign in with the test account, open one specific share, and read a small file. Confirm the content and that no other share was used as a fallback.</p>
<p><strong>Check:</strong> The named file opens from the remote client.</p>
<h3>Step 5: Restart and repeat</h3>
<p>Restart the Tailscale client, reconnect on cellular, and open the same file again. Note whether the NAS Tailscale service starts automatically after a reboot.</p>
<p><strong>Check:</strong> Access survives a client restart.</p>
<h2>Verify the result</h2>
<p>With Wi-Fi off, the client reaches the NAS over Tailscale, opens the named share, and reads the test file after a client restart.</p>
<h2>Rollback</h2>
<p>Remove the test client from Tailscale or sign it out after verification. Leave existing shares and accounts unchanged.</p>
<p><a href="/#consult">Ask for help with this exact symptom</a> · <a href="/guides/">All troubleshooting guides</a></p>
