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

Serve an existing Hugo site from one Tailscale address so another device on the same tailnet can review it privately. This skill covers draft previews and does not change production deployment, DNS, or firewall rules.

## When to use

Use when a user wants to access a local Hugo site or draft from another device over Tailscale.

Do not use for:

- Publishing a Hugo site
- Exposing a development server to the public internet
- Configuring Tailscale, ACLs, or firewall rules

## Prerequisites

- Hugo is installed and available to the runtime's command execution capability.
- The Hugo site path is known.
- The host is connected to the user's Tailscale network.
- The host's Tailscale IPv4 address is known, or can be retrieved with the runtime's command execution capability using `tailscale ip -4`.
- The reviewing device is connected to the same tailnet.

## How to run

Run Hugo with the runtime's command execution capability as a background process, binding directly to the host's Tailscale IPv4 address:

```bash
hugo server \
  --source <site> \
  --bind <tailscale-ip> \
  --port 1313 \
  --baseURL http://<tailscale-ip>:1313 \
  --appendPort=false \
  --buildDrafts
```

Use `--buildDrafts` when the site or post has `draft: true`. Omit it only when serving published content. If port `1313` is occupied, choose another port and use the same port in both `--port` and `--baseURL`.

## Procedure

1. **Inspect the site.** Use the runtime's file-reading capability to check the site's configuration and the target post's front matter. Confirm whether drafts must be included.
2. **Find the Tailscale address.** Use the runtime's command execution capability with `tailscale ip -4`, or use a verified Tailscale IPv4 address supplied by the user. Do not guess the address.
3. **Start Hugo narrowly.** Use the runtime's command execution and background-process capabilities to run the command above. Replace `<site>`, `<tailscale-ip>`, and the port only with verified values.
4. **Wait for readiness.** Inspect the background process output with the runtime's process or job-management capability until Hugo reports that the web server is available and shows the requested Tailscale bind address.
5. **Verify locally.** Use the runtime's command execution capability with `curl --fail --silent --show-error` against `http://<tailscale-ip>:<port>/`. For a draft, also request its rendered route and confirm HTTP success.
6. **Give the access URL.** Report the exact URL, including the port, and explain that the reviewing device must be connected to the same tailnet.
7. **Keep the server running.** Leave the background process running while the user reviews the site. Stop it only when the user asks or when the preview is no longer needed.

## Pitfalls

- Tailscale is a VPN. Binding to a Tailscale address provides private tailnet access; it is not a public deployment.
- Do not bind to `0.0.0.0` when the goal is Tailscale-only access. Do not substitute `127.0.0.1`, because another device cannot reach a loopback address on the host.
- Pair a port-bearing `--baseURL` with `--appendPort=false` to avoid duplicate port rewriting.
- A post with `draft: true` will not appear unless Hugo runs with `--buildDrafts`.
- A successful local request does not prove that a peer device can connect. If the peer cannot connect, check that both devices are on the same tailnet and that Tailscale policy permits the connection.
- Do not treat an exited Hugo process as a running preview. Read its output, correct the cause, and verify again.

## Verification

The preview is ready when all of these are true:

- Hugo reports the web server available on the requested Tailscale IPv4 address and port.
- `curl` returns HTTP success for the site root.
- The requested draft route returns HTTP success when drafts are enabled.
- The access URL uses the Tailscale IPv4 address rather than `localhost`, `127.0.0.1`, or `0.0.0.0`.
- The background process remains running for the duration of the review.

## Runtime mappings

This skill is runtime-neutral. Use the equivalent built-in capability provided by the agent runtime:

| Need | Hermes | Codex | Claude Code |
| --- | --- | --- | --- |
| Inspect files | `read_file` | Built-in file tools | Built-in file tools |
| Run commands | `terminal` | Shell/command execution | Shell/command execution |
| Manage the Hugo process | Background `terminal` plus `process` | Background shell process and native job controls | Background shell process and native job controls |

Do not require a tool literally named `read_file`, `terminal`, or `process` when another supported runtime provides the equivalent capability under a different name.
