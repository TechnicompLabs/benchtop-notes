# Pre-Packaged Environment

## CLI Utilities
* yq: command-line YAML, JSON, XML, CSV and properties processor — https://github.com/mikefarah/yq
* pandoc: command-line document conversion format

## Flatpak Management
* Bazaar Application Store
* Warehouse for Flatpak management
* Flatseal (permissions manager)

## Nix
Consider as per-user application environment (instead of distrobox).

## Backup
* restic — A modern backup program for your files

## Tailscale / NetworkManager Integration

Tailscale can integrate with NetworkManager so that VPN status appears in the GNOME network indicator and nm-applet.

### Current State
Tailscale runs as a standalone daemon (`tailscaled`) and manages its own `tailscale0` WireGuard interface. NetworkManager is not aware of this interface by default.

### Integration Options
1. **Tell NetworkManager to ignore the tailscale interface** (simplest, most reliable):
##### /etc/NetworkManager/conf.d/90-tailscale.conf
    [keyfile]
    unmanaged-devices=interface-name:tailscale*

2. **Use Tailscale's built-in NetworkManager support** (experimental):
Tailscale 1.52+ supports `--netfilter-mode=nodivert` and can optionally create a NetworkManager connection profile. This is still evolving; check Tailscale docs for current status.

3. **GNOME Extension**: The GSConnect-style approach — a GNOME Shell extension that shows Tailscale status, exit node selection, and connection state in the quick settings panel. See: https://github.com/maxgallup/tailscale-gnome-qs

## rclone
Mount nearly any remote storage service onto your local machine; great for multi-machine setups.
