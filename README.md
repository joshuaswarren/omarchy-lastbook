# omarchy-lastbook

[Lastbook](https://github.com/Immortal-Protocols/lastbook) is an offline survival library with maps, video, PDF and a local AI chat.
Upstream ships a deb for Ubuntu on x86_64. This repo builds it for Omarchy on x86_64 and on Apple Silicon (aarch64).

## Install

```sh
curl -fsSL https://raw.githubusercontent.com/joshuaswarren/omarchy-lastbook/main/omarchy-install-lastbook | bash
```

The script builds the package from the PKGBUILD in this repo (llama.cpp with Vulkan compiles on your machine, 10 to 20 minutes), installs it with pacman,
and opens Lastbook. After that, Lastbook is in the app launcher (Super + Space). First run opens the setup screen: pick a state and a library.

## What the package holds

- `/opt/lastbook`: the app as upstream ships it, plus `lastbook-llm`, built from llama.cpp b12032 with the Vulkan backend. The local AI runs on the GPU.
- `/usr/bin/lastbook` and `/usr/share/applications/lastbook.desktop`.
- Sources are pinned by commit and by SHA-256. The manifest upstream changes over time, so it has no checksum.

## Build by hand

```sh
git clone https://github.com/joshuaswarren/omarchy-lastbook && cd omarchy-lastbook
makepkg --syncdeps --install
```

## Chat on the Omarchy engine (Apple silicon)

On an Apple silicon Mac running Omarchy with `omarchy-mlx` installed, `/usr/bin/lastbook` starts the Omarchy engine instead of the bundled runner:
it serves the pinned catalog model `gemma-4-e4b-it-4bit` through `mlx-omarchy-serve` on loopback port 18951 (about 5 GiB of memory),
waits for it to come up, and starts the app pointed at that server. The bundled llama.cpp runner stays off. The server keeps
running between launches; `lastbook-omarchy-stop` ends it. Without `omarchy-mlx`, the menu entry works exactly as before.
