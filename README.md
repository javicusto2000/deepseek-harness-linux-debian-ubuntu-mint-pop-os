
### 🤝 Contributing & Community
This project was born out of a passion for bringing cutting-edge AI tools to the Linux ecosystem natively. Whether you are running a multi-boot local AI rig, testing models with `llama.cpp` or Ollama, or just love Debian, you are welcome here. 

If you find a bug, want to improve the patch script for other distributions (like Fedora/Arch), or have suggestions for better desktop integration, feel free to open an **Issue** or submit a **Pull Request**. Let's keep the Linux AI ecosystem thriving!


### 🛡️ Security, Transparency, and Supply Chain
Trust is paramount in open-source AI tooling. This repository is built on a philosophy of absolute transparency:
* **Zero Source Code Tampering:** We do not touch, modify, or distribute any proprietary logic, weights, or AI models. The build process strictly fetches the untouched source from the official DeepSeek AI repository.
* **Auditable Patching:** The `patch-linux.mjs` script is provided fully open-source and heavily commented. You can read exactly what it modifies before executing it.
* **Cryptographic Verification:** Every pre-compiled binary provided in the Releases section is hashed with SHA256. We strongly encourage all users to verify the checksum before installation to ensure the integrity of the package.


* ### ⚙️ Under the Hood: The Universal Linux Patch (`patch-linux.mjs`)
The official upstream repository hardcodes its build pipelines strictly for `mac-arm64`, `mac-x64`, and `win-x64`. To achieve a clean, native Debian package without altering the core upstream source, this repository utilizes a custom runtime injector (`patch-linux.mjs`). 

Here is what the patch dynamically handles during the build process:
1. **Toolchain Bypass:** Intercepts and overrides Apple/Microsoft specific code-signing policies (`desktop-package-environment.mjs`) that normally halt Linux compilations.
2. **ELF & pnpm Compatibility:** Injects `linux-x64` support directly into the pnpm orchestrator and TypeScript type definitions (`package-target.ts`), ensuring the Electron binaries are correctly fetched for the Linux architecture.
3. **Electron-Builder Overrides:** Forces the generation of a `.deb` package (instead of default AppImages) and injects the critical `t64` ABI dependencies (`libasound2t64`, `libgtk-3-0t64`) required by modern systems like Debian 13 (Trixie) and Ubuntu 24.04.
4. **Clean Executable Renaming:** Registers the binary cleanly as `deepseek-harness` in `/opt/`, preventing the messy internal `@deepseek-aidsh-desktop` naming conventions.


### 🖥️ Flawless Desktop Environment Integration
Unlike generic Linux builds that often result in broken window associations or blurry XWayland rendering, this release has been meticulously optimized for modern Linux desktop environments, particularly **KDE Plasma 6** and **GNOME** under **Wayland**.

* **Native Wayland Execution:** The `.desktop` entry is explicitly pre-configured with `--enable-features=UseOzonePlatform --ozone-platform=wayland` flags. This forces Electron to bypass the legacy X11 translation layer, delivering zero-latency hardware acceleration and crisp font rendering.
* **Task Manager Grouping Fix:** We resolved the notorious "Generic Wayland Icon (W)" bug. By strictly mapping the internal Electron `app.name` to the `StartupWMClass` (`deepseek-ai-dsh-desktop`), your OS task manager will instantly recognize the process, correctly grouping the active window under the official high-resolution DeepSeek icon.


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
