# g2ray Codespace Runner

This is the repository you push to GitHub and create a Codespace from.

It keeps the same basic idea as the original `g2ray` project, but moves startup out of browser attach terminals and into the Codespaces container lifecycle:

- `postStartCommand` starts Xray automatically when the Codespace starts.
- Xray listens on port `443`.
- Port `443` is marked public when GitHub CLI auth is available.
- Runtime files are written to `.g2ray/` in the workspace.
- `.g2ray/vless.txt` contains the VLESS URL.
- `.g2ray/g2ray.log` contains startup output and heartbeat lines.

Stopping the Codespace stops the forwarded port, so the VLESS config stops working. Starting the same Codespace again brings it back.

## Use

1. Create a new GitHub repository.
2. Copy this folder into it and push.
3. Create or start a Codespace for that repo.
4. Read the VLESS URL from `.g2ray/vless.txt`, or use the controller app in `g2ray-control-center`.

The default UUID is intentionally compatible with the controller defaults:

```text
550e8400-e29b-41d4-a716-446655440000
```

Change it before real use by adding a Codespaces secret or environment variable named `VLESS_UUID`, then set the same UUID in the controller GUI.

## Runtime Environment Variables

| Variable | Default | Meaning |
| --- | --- | --- |
| `VLESS_UUID` | `550e8400-e29b-41d4-a716-446655440000` | Client UUID. |
| `VLESS_CONNECT_HOST` | generated Codespaces host | Host/IP used before `:443` in the VLESS URL. |
| `VLESS_FRONT_IP` | empty | Optional fallback front IP if `VLESS_CONNECT_HOST` is not set. |
| `VLESS_PATH` | `/` | XHTTP path. |
| `VLESS_LABEL` | `g2ray-<codespace>` | URL fragment label. |
| `XRAY_LOG_LEVEL` | `warning` | Xray log level. |
| `G2RAY_HEARTBEAT_SECONDS` | `30` | Heartbeat interval written to `.g2ray/g2ray.log`. |

## Useful Commands Inside The Codespace

```bash
g2ray-config
g2ray-status
tail -f .g2ray/g2ray.log
```

## Notes

This does not bypass GitHub Codespaces billing, quotas, idle behavior, or terms. Use it only on accounts and repositories you own or have permission to operate.
