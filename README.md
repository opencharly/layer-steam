# steam

The Steam gaming client plus the gamescope nested compositor for OpenCharly
images, on a Sway/XWayland desktop.

The `steam` candy installs the Steam client and the gamescope micro-compositor
from the Fedora + RPM Fusion nonfree repos, and pins
`STEAM_RUNTIME_PREFER_HOST_LIBRARIES=0` so Steam uses its own bundled runtime
instead of host libraries.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `steam` |
| Binaries | `/usr/bin/steam`, `/usr/bin/gamescope` |
| Packages | `steam`, `gamescope` (Fedora + RPM Fusion nonfree) |
| Env | `STEAM_RUNTIME_PREFER_HOST_LIBRARIES=0` |
| Security | `shm_size: 1g` |
| Volume | `steam-data` → `~/.local/share/Steam` |
| Requires | [`pod-sway`](https://github.com/opencharly/pod-sway) |
| Service / port | none — needs a Sway desktop box |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list (requires the
`nvidia` base for RPM Fusion nonfree):

```yaml
sway-browser-vnc-steam:
  candy:
    base: nvidia
    candy:
      - sway-desktop-vnc
      - '@github.com/opencharly/layer-steam:v2026.243.0507'
```

Steam is an X11 application, so it needs XWayland. Gamescope is a nested Wayland
compositor; use it in Steam launch options:
`gamescope -W 1920 -H 1080 -r 60 -- %command%`.

The candy's `plan:` asserts the Steam launcher, the gamescope binary, both RPM
packages, and the runtime env pin.

## First login

Steam Guard requires interactive login. Connect via the VNC desktop, launch
Steam, and log in manually; auth tokens persist in the `steam-data` volume.

## Layout

- `charly.yml` — the `steam:` candy entity (the `require:`, `env:`, `security:`,
  `volume:`, the Fedora package section, and the `check:` probes) and the
  embedded `steam-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:steam`
- Compositor dependency: `/charly-selkies:sway`, `/charly-selkies:sway-desktop-vnc`
- GPU base: `/charly-distros:cuda`, `/charly-distros:nvidia`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
