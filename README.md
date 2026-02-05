# taildrive-mount-davfs-systemd

Automatically mount a WebDAV share when Tailscale is running, and unmount it when Tailscale goes down.

## Features

- ✅ Mounts WebDAV share when Tailscale starts
- ✅ Unmounts WebDAV share when Tailscale stops
- ✅ Checks every 2 seconds via systemd timer
- ✅ Non-interactive mounting (no password prompts)

## Installation

### From Source
```bash
git clone https://github.com/zfogg/taildrive-mount-davfs-systemd
cd taildrive-mount-davfs-systemd
makepkg -si
```

## Configuration

After installation, edit to customize:

- `/usr/bin/do-mount-webdav` - WebDAV mount URL and mount point
- `/usr/lib/systemd/system/ensure-webdav-mounted.timer` - Check interval (default: 2 seconds)

Default configuration:
- **WebDAV URL**: `http://100.100.100.100:8080`
- **Mount point**: `/mnt/tailscale`
- **Check interval**: 2 seconds

## How It Works

1. **Timer**: Systemd timer runs every 2 seconds
2. **Service**: Checks if Tailscale is running
3. **Mount**: If Tailscale is up and not mounted, mounts the WebDAV share
4. **Unmount**: If Tailscale is down and mounted, unmounts the share
5. **Pre-Stop Hook**: Unmounts before Tailscale stops to prevent hangs

## Files

- `src/do-mount-webdav` - WebDAV mount script
- `src/ensure-webdav-mounted` - Status check and mount/unmount logic
- `systemd/ensure-webdav-mounted.service` - Systemd service
- `systemd/ensure-webdav-mounted.timer` - Systemd timer (2-second interval)
- `systemd/tailscaled-unmount-webdav.conf` - Drop-in to unmount before Tailscale stops

## Requirements

- Tailscale
- davfs2
- systemd
