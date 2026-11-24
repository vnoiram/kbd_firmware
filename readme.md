# KBD firmware

## How to build

## 1. Setting Up Your QMK Environment

Please see https://docs.qmk.fm/#/newbs_getting_started and set up 1 to 3.

## 2. Getting source files

Please get source files of `qmk/qmk_firmware` and `vial-kb/vial-qmk`
```sh
make git-submodule
```

## 3. Building firmwares

### for VIA

```sh
make qmk-clean
kb=crkbd make qmk-init
kb=crkbd kr=rev4_1/standard km=via make qmk-compile
```
A built data will be stored on `keyboards/crkbd/qmk/qmk_firmware/.build`\
Please change `kb`, `kr` and `km` when build other.

### for Vial
```sh
make vial-qmk-clean
kb=crkbd make vial-qmk-init
kb=crkbd kr=rev4_1/standard km=vial make vial-qmk-compile
```
A built data will be stored on `keyboards/crkbd/vial-kb/vial-qmk/.build`\
Please change `kb`, `kr` and `km` when build other.

### All cleaning and building
```sh
make update-all
```

## Building with an existing qmk_firmware checkout (without submodules)

If you already have your own `qmk_firmware` clone (e.g. a fork), you can build
from it directly instead of fetching another copy via `make git-submodule`.
The submodules in `src/` are left uninitialized in this setup.

Assumed layout:

```
/path/to/workspace/
├── kbd_firmware/   # this repository
└── qmk_firmware/   # your qmk_firmware clone (already set up with `qmk setup`)
```

### 1. Link the keyboard directory into qmk_firmware

`keyboards/crkbd` already exists in upstream QMK, so link it under a
different name (here `crkbd_foostan`).

```sh
cd /path/to/workspace/qmk_firmware
ln -s "$(cd ../kbd_firmware && pwd)/keyboards/crkbd/qmk/qmk_firmware" keyboards/crkbd_foostan
```

To keep the link out of `git status` in qmk_firmware without touching its
tracked `.gitignore`, add it to the local exclude file:

```sh
echo 'crkbd_foostan' >> .git/info/exclude
```

### 2. Build

Run in `qmk_firmware`. Use `qmk compile` rather than plain `make` so that the
ARM toolchain installed by `qmk setup` is on `PATH` (rev4_x / rev5_0 are RP2040).

```sh
qmk compile -kb crkbd_foostan/rev4_1/standard -km via
qmk compile -kb crkbd_foostan/rev4_1/mini -km via
```

The firmware (`crkbd_foostan_rev4_1_standard_via.uf2` etc.) is written to the
top of `qmk_firmware`.

Personal keymaps go in
`keyboards/crkbd/qmk/qmk_firmware/keymaps/<name>/` of this repository and are
built with `-km <name>`.

### Managing your own changes

Each repository is managed independently. A typical remote setup is:

```sh
cd /path/to/workspace/kbd_firmware
git remote add ups git@github.com:<your-account>/kbd_firmware.git   # your fork
# origin stays at https://github.com/foostan/kbd_firmware
```
