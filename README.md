# aptrepo

APT repository for LV2 plugins, served via GitHub Pages from the `gh-pages` branch.

Packages are published automatically when plugin projects (e.g. [connie](https://github.com/jamesmittlerii/connie)) pass CI on `main`.

## Setup (one-time)

1. **GitHub Pages** — repo **Settings → Pages → Branch: `gh-pages`**, folder `/ (root)`.
2. **Secret on each plugin repo** — add `APTREPO_PUSH_TOKEN`: a fine-grained PAT with **Contents: Read and write** on this repository.

The `gh-pages` branch is created automatically on the first successful release. Do not commit to `gh-pages` by hand.

## Install (Debian 12 / Raspberry Pi OS Bookworm)

**Desktop amd64:**

```bash
echo "deb [trusted=yes arch=amd64] https://jamesmittlerii.github.io/aptrepo/ bookworm main" \
  | sudo tee /etc/apt/sources.list.d/jamesmittlerii-lv2.list
```

**Raspberry Pi 64-bit (Pi OS / Debian arm64)** — use `arch=arm64` so apt does not look for armhf packages we do not publish:

```bash
echo "deb [trusted=yes arch=arm64] https://jamesmittlerii.github.io/aptrepo/ bookworm main" \
  | sudo tee /etc/apt/sources.list.d/jamesmittlerii-lv2.list
```

Then:

```bash
sudo apt update
sudo apt install lv2-connie
```

Restart your DAW or rescan LV2 plugins. Verify:

```bash
sudo apt install lilv-utils   # optional
lv2ls | grep -i connie
```

## Layout

```text
gh-pages/                 ← served at https://jamesmittlerii.github.io/aptrepo/
  conf/distributions      ← reprepro config
  dists/bookworm/         ← package index
  pool/                   ← .deb files
```

## Adding another LV2

Use the same `APTREPO_PUSH_TOKEN` from the plugin’s release workflow. All packages share this repo and codename (`bookworm`).
