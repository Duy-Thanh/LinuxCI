# LinuxCI

> **Automated, Untouched & Always Up-to-Date Linux Kernel Builds**

[![Build Kernel](https://github.com/Duy-Thanh/LinuxCI/actions/workflows/build-kernel.yml/badge.svg)](https://github.com/Duy-Thanh/LinuxCI/actions)
[![GitHub Release](https://img.shields.io/github/v/release/Duy-Thanh/LinuxCI?color=blue&label=Latest%20Kernel)](https://github.com/Duy-Thanh/LinuxCI/releases)
[![License: GPL-2.0](https://img.shields.io/badge/License-GPL%20v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)

Stop waiting for local compilation (`make -j$(nproc)`) and roasting your CPU! **LinuxCI** is a fully automated continuous integration pipeline that fetches, builds, and releases the **latest stable Linux Kernel** directly from [kernel.org](https://kernel.org).

No bloated distro patches. No proprietary lock-ins. Just pure generic kernel binaries ready to boot on any modern Linux distribution.

---

## Features

* **Auto-Sync:** Daily checks for new mainline stable releases on `kernel.org`.
* **Pure Modern Config:** Built using **Arch Linux's official x86_64 base configuration** for maximum hardware support and performance.
* **Generic Binaries:** Packaged as raw `bzImage` and `modules.tar.xz`—compatible with Arch, Fedora, Debian, Ubuntu, Gentoo, Void, and more.
* **Transparent & Secure:** 100% open pipeline via GitHub Actions. SHA256 checksums provided for every single release.
* **Optimized Builds:** Unnecessary debug symbols (`DEBUG_INFO`, `BTF`) are disabled to ensure compact file sizes and optimal execution speed.

---

## How to Install

Download the latest release binaries from the [Releases Page](https://github.com/YOUR_GITHUB_USERNAME/LinuxCI/releases).

### 1. Extract Kernel Modules

```bash
# Replace X.Y.Z with the downloaded release version
sudo tar -xJf modules-X.Y.Z-linuxci.tar.xz -C /
```

### 2. Copy Kernel Image

```bash
sudo cp bzImage-X.Y.Z /boot/vmlinuz-X.Y.Z-linuxci
```

### 3. Update bootloader
### For GRUB users

```bash
# Debian / Ubuntu / Mint
sudo update-grub

# Arch / Fedora / RHEL
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

### For systemd-boot users:
Add a new entry file in `/boot/loader/entries/linuxci.conf`:

```ini,TOML
title   Linux Kernel X.Y.Z (LinuxCI)
linux   /vmlinuz-X.Y.Z-linuxci
options root=UUID=YOUR-ROOT-PARTITION-UUID rw
```

Reboot your machine and select the new kernel version!

## Verification

Always verify the integrity of downloaded files using the provided SHA256 checksums:

```bash
sha256sum -c SHA256SUMS
```

## Project Architecture

```
[ kernel.org API ] ──(Daily Cron)──> [ GitHub Action Workflow ]
                                             │
                                   Fetch Arch Linux .config
                                             │
                                   Strip Debug Symbols & Certs
                                             │
                                  Compile bzImage & Modules
                                             │
                                    Generate SHA256SUMS
                                             │
                                     [ GitHub Release ]
```

## License
This project automation script is released under the MIT License. The compiled Linux Kernel binaries are distributed under the terms of the **GNU General Public License v2.0** as set by the upstream Linux Kernel project.