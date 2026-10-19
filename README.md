# NordDuck Linux - custom live OS

A custom bootable live ISO of [Void Linux](https://voidlinux.org/) with the
**i3** tiling window manager preconfigured, built with
[void-mklive](https://github.com/void-linux/void-mklive). Boots into the
Nord-themed i3 desktop with a full installer (`nordduck-installer`, branded
from void-installer) to permanently install the entire custom system.

## What you get

Boot the ISO (USB stick: `dd if=nordduck-live-*.iso of=/dev/sdX bs=4M status=progress`)
and you land directly in the i3 desktop (autologin).

- **Desktop**: i3 (Nord color scheme, gaps, smart borders, app workspaces,
  dropdown scratchpad terminal), polybar (with clickable **apps** module),
  picom compositor, dark void wallpaper, **rofi app search** (`Super+d`),
  dunst notifications — all dark-themed, including Qt apps (VLC, octoxbps)
- **Terminal**: Alacritty (transparent, Nord) + xterm fallback
- **Apps**: Firefox, Thunar (file manager), nnn (CLI file browser),
  VLC, pavucontrol
- **Audio**: PipeWire + WirePlumber + ALSA/Pulse bridges
- **Network**: NetworkManager + nm-applet tray icon
- **Software center**: octoxbps (GUI package manager) + full xbps CLI
  (`sudo xbps-install -S <pkg>`)
- **System**: runit services (dbus, elogind, polkitd, NetworkManager),
  eudev, elogind sessions, sudo, installable via `nordduck-installer`

Login/root password: `voidlinux` (user `anon` autologins; kernel args
`live.user=NAME` / `live.autologin` change that).

## Keybindings

| Key            | Action                            |
|----------------|-----------------------------------|
| `Super+Enter`  | terminal (alacritty)              |
| `` Super+` ``  | dropdown terminal (scratchpad)    |
| `Super+d`      | search & launch apps (rofi combi)  |
| `Super+Shift+d`| app grid (rofi drun, icons)        |
| `Super+e`      | Thunar file manager               |
| `Super+q`      | kill focused window               |
| `Super+1..9`   | switch workspace (apps auto-open on their own) |
| `Super+f`      | fullscreen                        |
| `Super+r`      | resize mode (vim/arrow keys, Shift = fine) |
| `Super+Shift+c`| reload config                     |
| `Print`        | screenshot region -> clipboard    |
| `XF86Audio*`   | volume / mute / brightness        |

## Rebuilding

```sh
sudo bash build.sh          # produces nordduck-live-<date>.iso, logs to build.log
```

Prerequisites: a Linux host (root), `curl`, `tar`, `python3`; on Fedora the
script installs `dosfstools`/`e2fsprogs` itself. The script fetches the static
`xbps` toolchain, so it runs on any distro. Note: this build uses plain-HTTP
Void mirrors because the sandboxed host blocks TLS from the static xbps
binary; the final ISO itself ships the official HTTPS repositories.

## Layout

```
build.sh                    entry point (run with sudo)
buildscripts/
  gen-wallpaper.py          generates the Nord gradient wallpaper
  postsetup.sh              rootfs tweaks before the initramfs is built
include/                    overlay copied into the image rootfs
  etc/motd                  login banner
  etc/pipewire, etc/alsa    pipewire/ALSA integration
  etc/skel/                 dotfiles: i3, polybar, alacritty, picom, GTK
void-mklive/                upstream live-image generator
```
