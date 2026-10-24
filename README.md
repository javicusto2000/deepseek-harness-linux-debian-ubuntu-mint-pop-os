# DeepSeek Harness Desktop - Unofficial Linux Build 🐧

[![Platform: Linux](https://img.shields.io/badge/Platform-Linux-yellow.svg)](https://kernel.org)
[![Display: Wayland](https://img.shields.io/badge/Display-Wayland-blue.svg)](https://wayland.freedesktop.org/)
[![DE: KDE Plasma](https://img.shields.io/badge/DE-KDE_Plasma-informational.svg)](https://kde.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

> ⚡ **Upstream Sync Status:** Tracking the latest official release **v0.2.1-rc.3** (Compiled and packaged just hours after the official DeepSeek update, including the newest V4.1-Flash model support).

A community-driven, native Linux packaging of the official [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness). This project provides a pre-compiled `.deb` installer strictly optimized for modern Debian-based distributions running **Wayland**, bringing the full DeepSeek AI desktop experience to the Linux ecosystem with proper window management and system integration.

## 🌟 Why this project? (Features & Benefits)

The official DeepSeek Harness repository currently focuses its build pipeline on macOS and Windows. Compiling it for Linux directly out-of-the-box yields missing icons, broken window associations (generic Wayland icons in Task Managers), and unsupported macOS/Windows signing policies. 

This repository solves these issues transparently by providing:
*   **Native Wayland Rendering:** Forces Electron's Ozone platform for crisp, hardware-accelerated UI without XWayland blurring.
*   **Flawless KDE/GNOME Integration:** Properly maps the `StartupWMClass` to ensure the application groups correctly in your Task Manager with the official high-resolution DeepSeek icon.
*   **Security & Transparency:** We do not alter the core DeepSeek source code. We merely inject a universal Linux compatibility patch (`patch-linux.mjs`) during the build process to bypass Apple/Microsoft code-signing policies and correctly configure `electron-builder` for `.deb` generation.
*   **Reproducible Builds:** The entire step-by-step build guide is open and documented below. You can trust our provided `.deb` binary (verified via SHA256), or compile it yourself in minutes.

## 💻 System Requirements

**Important:** This `.deb` package was compiled on **Debian 13 (Trixie)** and relies on the modern `t64` ABI transition libraries.
*   **Supported OS:** Debian 13+, Ubuntu 24.04 LTS+, Linux Mint 22+, or any modern derivative using `t64` libraries (e.g., `libasound2t64`, `libgtk-3-0t64`).
*   **Display Server:** Optimized for **Wayland**. *(Note: X11 users can still use it, but will need to remove the `--ozone-platform=wayland` flag from the `/usr/share/applications/deepseek-harness.desktop` file after installation).*

---

## 🚀 Quick Install (Pre-compiled Binary)

1. **Download the latest `.deb`** from the [Releases page](../../releases).
2. **Verify the checksum** for security (Replace `395a9be5b0b22403b095d2ca09e589b8f0f46a948fe62f06ceaeead208a05be0  deepseek-harness-0.2.1-rc.3-linux-amd64-unsigned.deb` with the one provided in the release notes):
   ```bash
   echo "395a9be5b0b22403b095d2ca09e589b8f0f46a948fe62f06ceaeead208a05be0  deepseek-harness-0.2.1-rc.3-linux-amd64-unsigned.deb deepseek-harness-0.2.1-rc.3-linux-amd64-unsigned.deb" | sha256sum --check
