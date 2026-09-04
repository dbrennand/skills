---
name: hugo-tailscale-preview
description: Serve Hugo drafts through a Tailscale-only preview server.
version: 0.1.0
author: Daniel Brennand (dbrennand), Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  tags: [Hugo, Tailscale, Preview]
  related_skills: []
---

# Hugo Tailscale preview

Serve an existing Hugo site from one Tailscale address so another device on the same tailnet can review it privately. This skill covers draft previews.

## When to use

Use when a user wants to access a local Hugo site or draft from another device over Tailscale.

## Prerequisites

- Hugo is installed and available on `PATH`.
- The Hugo site path is known.
- The host is connected to the user's Tailscale network.
- The host's Tailscale IPv4 address is known or can be retrieved with `tailscale ip -4`.
- The reviewing device is connected to the same tailnet.

## How to run

Run Hugo as a background process, binding directly to the host's Tailscale IPv4 address and including drafts:

```bash
hugo server \
  --source <site> \
  --bind <tailscale-ip> \
  --port 1313 \
  --baseURL http://<tailscale-ip>:1313 \
  --appendPort=false \
  --buildDrafts
```

The `--buildDrafts` flag includes posts with `draft: true`. If port `1313` is occupied, choose another port and use the same port in both `--port` and `--baseURL`.

## Procedure

1. **Get the Tailscale address.** Run `tailscale ip -4` and use the returned IPv4 address for `<tailscale-ip>`.
2. **Start Hugo.** Run the command above as a background process, replacing `<site>`, `<tailscale-ip>`, and the port with the relevant values. Keep `--buildDrafts` enabled so draft posts are included.
3. **Share the access URL.** Report the exact URL, including the port, and explain that the reviewing device must be connected to the same tailnet.
4. **Keep the server running.** Leave the background process running while the user reviews the site. Stop it only when the user asks or when the preview is no longer needed.

## Pitfalls

- Tailscale is a VPN. Binding to a Tailscale address provides private tailnet access.
- Do not bind to `0.0.0.0` when the goal is Tailscale-only access. Do not substitute `127.0.0.1`, because another device cannot reach a loopback address on the host.
- Pair a port-bearing `--baseURL` with `--appendPort=false` to avoid duplicate port rewriting.
- A post with `draft: true` will not appear unless Hugo runs with `--buildDrafts`.
