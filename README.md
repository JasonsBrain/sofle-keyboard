# ZMK Config — Sofle (nRF52840) Starter

This is a minimal **zmk-config** repo you can upload to GitHub to build ZMK firmware for a **Sofle split** using **nRF52840 Pro Micro–style controllers** (defaults to `nice_nano_v2`).

## Quick start
1. Create a new GitHub repository (public or private).
2. Download this zip, unzip it, and upload all files into your repo root.
3. Go to **Actions** → enable workflows → wait for the build to start.
4. After the build finishes, download the **firmware artifact** and you’ll get:
   - `sofle_left-nice_nano_v2.uf2`
   - `sofle_right-nice_nano_v2.uf2`
5. Put each controller into bootloader (double‑tap reset), it should mount as a drive, then drag the matching `.uf2` onto each half.

## How to "tell ZMK" what you have
You declare two things in the workflow file:
- **BOARD** — your controller (default: `nice_nano_v2`).
- **SHIELD** — your keyboard half (`sofle_left`, `sofle_right`).

These are set in: `.github/workflows/build.yml`

If your controller is **not** a nice!nano v2, change `board:` in `build.yml` to match your board. Common ones:
- `nice_nano` (older v1)
- `nrfmicro_13`
- `bluemicro840_v1`

If the workflow errors with *“Unknown board”*, your controller name is different — tell me the exact model printed on the PCB or product page and we’ll swap it in.

## Keymap
This starter does **not** override the keymap; it uses the Sofle shield’s default so you can flash right away and test typing.
When you’re ready to customize, add a file at `config/sofle.keymap` and we’ll fill it in together.

## Local builds (optional, advanced)
If you prefer local builds, you can use `west` with this repo:
```
west init -l config
west update
west build -s zmk/app -b nice_nano_v2 -- -DSHIELD=sofle_left -DZMK_CONFIG=$PWD/config
west build -s zmk/app -b nice_nano_v2 -- -DSHIELD=sofle_right -DZMK_CONFIG=$PWD/config
```
Your UF2s will be in `build/zephyr/` each time.

## Troubleshooting
- **Shield not found** – If you see an error about `sofle_left`/`sofle_right` not found, your ZMK version might be old. This repo pins to ZMK `main`. Make sure `config/west.yml` is present and Actions successfully runs `west update`.
- **Board not found** – Change `board:` in the workflow to match your controller (see list above).
- **UF2 not created** – Some boards output `.hex` instead of `.uf2`. The workflow copies whatever ZMK built into `build-artifacts/`. Flash using the right method for your bootloader.

