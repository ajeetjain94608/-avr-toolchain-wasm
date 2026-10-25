# AVR toolchain for the browser — corresponding source

This repository is the **complete corresponding source** for the AVR compiler that the RoboQuest website and app
run in the browser (WebAssembly). It is published to meet the GNU General Public License version 3 (GCC, binutils)
and the GNU Lesser General Public Licenses (GMP, MPFR, MPC; the Arduino AVR core).

## What is in the shipped compiler

| Component | Version | Licence | Source here |
|---|---|---|---|
| GNU GCC (`cc1plus`) | 7.3.0 + Arduino's `atmel-patches-gcc.7.3.0-arduino2.patch` | GPL-3.0 (+ GCC Runtime Library Exception) | `sources/gcc-7.3.0.tar.xz`, `sources/atmel-patches-gcc.7.3.0-arduino2.patch` |
| GNU binutils (`as`, `ld`, `objcopy`, `readelf`) | 2.42 | GPL-3.0 | `sources/binutils-2.42.tar.xz` |
| GMP / MPFR / MPC (linked into cc1plus) | 6.2.1 / 4.1.0 / 1.2.1 | LGPL-3.0 | `sources/gmp-6.2.1.tar.xz`, `sources/mpfr-4.1.0.tar.xz`, `sources/mpc-1.2.1.tar.gz` |
| Arduino AVR core (precompiled `core.a`) | 1.8.7 | LGPL-2.1 | `sources/ArduinoCore-avr-1.8.7.tar.gz` |

AVR-LibC (Modified BSD licence) and libgcc (GPL-3.0 with the GCC Runtime Library Exception, built from the same GCC
source and patch) are taken unchanged from Arduino's `avr-gcc 7.3.0-atmel3.6.1-arduino7` toolchain.

Checksums of every archive: `scripts/SHA256SUMS`. Licence texts: `LICENSE` (GPL-3.0), `COPYING3.LIB` (LGPL-3.0),
`LGPL-2.1.txt`.

Programs compiled with this compiler are **not** covered by the GPL (GCC Runtime Library Exception).

## How to rebuild

In Linux (tested on Ubuntu 24.04 under WSL2) with build tools and Emscripten 4.0.10:

```bash
sudo apt install build-essential bison flex texinfo git python3 xz-utils wget m4
git clone https://github.com/emscripten-core/emsdk.git ~/emsdk && cd ~/emsdk && ./emsdk install 4.0.10 && ./emsdk activate 4.0.10

mkdir -p ~/rq-avr-wasm/src && cp sources/* ~/rq-avr-wasm/src/   # use these archives (checksums are verified)
bash scripts/build.sh all                                        # → out/<tool>.mjs + <tool>.wasm
```

`scripts/build.sh` is the exact recipe used for the published binaries (configure options, the fixes needed to build
with Emscripten, and the link flags). The precompiled Arduino core is built with `scripts/build-core.mjs` (flags in
`scripts/nativeToolchain.mjs`); `scripts/record-commands.mjs` records the compiler steps the app runs, and
`scripts/package-sysroot.mjs` packs the headers and libraries the in-browser compiler reads.

## Contact

Questions about this source: open an issue in this repository.
