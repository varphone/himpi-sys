himpi-sys
=========

Hi35XX MPI API for Rust unsafe bindings.

Requirements
------------

1. The target board toolchain must be installed and export to `PATH`:
    - hi3516ev200 - `arm-himix100-linux`.
    - hi3516ev300 - same as `hi3516ev200`.
    - hi3518ev200 - same as `hi3516ev200`.
    - hi3518ev300 - same as `hi3516ev200`.
    - hi3519av100 - `arm-himix200-linux`.
    - hi3531v100 - `arm-hisiv100nptl-linux`.
    - hi3559av100 - `aarch64-himix100-linux`.
2. The rust target must be installed for target board:
    - hi3516ev200 - `armv7-unknown-linux-musleabihf`.
    - hi3516ev300 - same as `hi3516ev200`.
    - hi3518ev200 - same as `hi3516ev200`.
    - hi3518ev300 - same as `hi3516ev200`.
    - hi3519av100 - `aarch64-unknown-linux-gnu`.
    - hi3531v100 - `arm-unknown-linux-musleabi`.
    - hi3559av100 - `aarch64-unknown-linux-gnu`.
3. Export `MPP_DIR` to directory that contains the `mpp-lib`.
4. Export `SYS_INCLUDE` to the directory that contains the system headers.
5. Export `SYS_LIBDIR` to the directory that contains the system libraries.

> The toolchain and the mpp-lib you can get from the BSP sdk.

Envionment Defaults
-------------------

### Hi3516EV200

The Hi3616EV300、Hi3616EV200、Hi3618EV300 use the same SDK.

```sh
export PATH=/opt/hisi-linux/x86-arm/arm-himix100-linux/bin:$PATH
export MPP_DIR=vendor/mpp-lib-Hi3516EV200_V1.0.1.0
export SYS_INCLUDE=/opt/hisi-linux/x86-arm/arm-himix100-linux/target/usr/include
```

### Hi3519AV100

```sh
export PATH=/opt/hisi-linux/x86-arm/arm-himix200-linux/bin:$PATH
export MPP_DIR=vendor/mpp-lib-Hi3519AV100_V2.0.2.0
export SYS_INCLUDE=/opt/hisi-linux/x86-arm/arm-himix200-linux/target/usr/include
```

### Hi3531V100

```sh
export PATH=/opt/hisi-linux-nptl/arm-hisiv100-linux/target/bin:$PATH
export MPP_DIR=vendor/mpp-lib-Hi3531V100_V1.0.D.0
export SYS_INCLUDE=/opt/hisi-linux-nptl/arm-hisiv100-linux/target/usr/include
```

### Hi3559AV100

```sh
export PATH=/opt/hisi-linux/x86-arm/aarch64-himix100-linux/bin:$PATH
export MPP_DIR=vendor/mpp-lib-Hi3559AV100_V2.0.2.0
export SYS_INCLUDE=/opt/hisi-linux/x86-arm/aarch64-himix100-linux/aarch64-linux-gnu/sys-include
```

Building
--------

To build the package, you must set cross compile environments first.

There is some preset in `.cargo/` can help you fasten setup the cross compile.

Example:

```sh
# Setup for Hi3559AV100 boards
cp .cargo/hi3559av100.toml .cargo/config
# or for link static libraries
cp .cargo/hi3559av100-static.toml .cargo/config
# Build the package ...
cargo b
```

> Make sure the `hi3559av100` feature is enabled in the Cargo.toml,
> The `hi3559av100` specified the target board, you can change to others,
> like: `hi3531v100`
> More features see the Cargo.toml.

To build with `arm-hisiv100-linux-uclibcgnueabi`:

```sh
# the nightly toolchain and rust-src must be installed.
rustup override set nightly
rustup component add rust-src
# the xargo command must be installed.
cargo install xargo
# if the above already, that should be work now.
RUST_TARGET_PATH=$(pwd) xargo [b|c|r|t] ...
```

> if RUST_TARGET_PATH not set, the xargo will raise
> "Error loading target specification..."

**Patch bindgen**:

The package use a patched bindgen to generate ffi bindings,
you must patch the crates.io to replace the bindgen with the patched version.

```toml
# Cargo.toml

[patch.crates-io]
bindgen = { git = "https://github.com/varphone/rust-bindgen.git", rev = "v0.53.2-sp1" }
```

Examples
--------

```rs
use himpi_sys::{HI_MPI_SYS_Init, HI_MPI_SYS_Exit};

fn main() {
    unsafe {
        println!("HI_MPI_SYS_Init() = {}", HI_MPI_SYS_Init());
        println!("HI_MPI_SYS_Exit() = {}", HI_MPI_SYS_Exit());
    }
}
```
