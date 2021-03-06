```
██╗░░░██╗███╗░░██╗███╗░░░███╗░█████╗░██╗███╗░░██╗████████╗░█████╗░██╗███╗░░██╗███████╗██████╗░
██║░░░██║████╗░██║████╗░████║██╔══██╗██║████╗░██║╚══██╔══╝██╔══██╗██║████╗░██║██╔════╝██╔══██╗
██║░░░██║██╔██╗██║██╔████╔██║███████║██║██╔██╗██║░░░██║░░░███████║██║██╔██╗██║█████╗░░██║░░██║
██║░░░██║██║╚████║██║╚██╔╝██║██╔══██║██║██║╚████║░░░██║░░░██╔══██║██║██║╚████║██╔══╝░░██║░░██║
╚██████╔╝██║░╚███║██║░╚═╝░██║██║░░██║██║██║░╚███║░░░██║░░░██║░░██║██║██║░╚███║███████╗██████╔╝
░╚═════╝░╚═╝░░╚══╝╚═╝░░░░░╚═╝╚═╝░░╚═╝╚═╝╚═╝░░╚══╝░░░╚═╝░░░╚═╝░░╚═╝╚═╝╚═╝░░╚══╝╚══════╝╚═════╝░

This repository is unmaintained since Lisk will migrate to a different length address format
(LIP0018) and the author is not planning to update this repository to the new address system.
```

# lisk-vanity

Generates a short Lisk address. The shorter the address, the longer it'll take to compute.

## Requirements

* [Rustup](https://rustup.rs/) or a different way to run Rust (stable)

## Installation from source

```
git clone https://github.com/webmaster128/lisk-vanity.git
cd lisk-vanity
```

To compile, we run the following commands Rust environment (`source $HOME/.cargo/env`):

```
cargo build --release
./target/release/lisk-vanity --version
```

## Getting started

For a list of `lisk-vanity` options, use `lisk-vanity --help`.

To search for an address of length 13 (suffix "L" expluded), run

```
$ lisk-vanity 13
Estimated attempts needed: 1844674
Tried 1060873 keys (~57.51%; 41497.1 keys/s)
Found matching account!
Private Key: fan bonus chronic like lobster ankle forum unusual hedgehog rich cruise craft
Address:     2702373550273L
```

Add `--gpu` to add GPU support:

```
$ lisk-vanity --gpu 12
Estimated attempts needed: 18446744
GPU platform NVIDIA Corporation NVIDIA CUDA
Using GPU device NVIDIA Corporation GeForce GTX 1080, OpenCL 1.2
Tried 7453113 keys (~40.40%; 709077.4 keys/s)
Found matching account!
Private Key: autumn expire topple rebel rebuild rack process digital possible decorate trigger stand
Address:     310696579609L
```

Set CPU thread to zero to use GPU only:

```
$ lisk-vanity --gpu --cpu-threads 0 12
Estimated attempts needed: 18446744
GPU platform NVIDIA Corporation NVIDIA CUDA
Using GPU device NVIDIA Corporation GeForce GTX 1080, OpenCL 1.2
Tried 14680064 keys (~79.58%; 651781.0 keys/s)
Found matching account!
Private Key: average depend juice skate pupil order lift mention foster modify uphold divert
Address:     483891294046L
```

Use `--generate-keypair` to generate raw Ed25519 keypair in libsodium format (32 bytes secret key + 32 bytes public key)

```
$ lisk-vanity  --gpu --generate-keypair --cpu-threads 0 12
Estimated attempts needed: 18446744
GPU platform NVIDIA Corporation NVIDIA CUDA
Using GPU device NVIDIA Corporation GeForce GTX 1080, OpenCL 1.2
Tried 32505856 keys (~176.21%; 682165.2 keys/s)
Found matching account!
Private Key: B977D77F7126BC1E07E03FC276A2AC711148DF5497643BE575B24402ADA03153FCF22269A265BC349932ED7EACD34504FBBC8D05B92E4E4DC24D7955757E0D5C
Address:     456618761412L
```

## Advances GPU settings

This project supports using your GPU to compute the address.
This utilizes OpenCL, so you'll need OpenCL installed and working.

This project is built with GPU support by default (cargo feature "gpu").

To enable GPU use, use the `--gpu` (or `-g`) option. To disable
use of your CPU, use `--cpu-threads 0` (or `-t 0`).

To change your GPU platform, use `--gpu-platform [index]`, where `[index]`
is the index of your GPU starting at 0.
To change your GPU device, use `--gpu-device [index]`.

## Common issues and troubleshooting

### OpenCL compilation on the AMD toolchain rocm hangs forever

This is a known compiler bug in rocm: https://github.com/RadeonOpenCompute/ROCm/issues/683.

You could potentially use Windows and AMD's Windows toolchain, which is not affected.

### Cannot find -lOpenCL

The rust compilation error

```
  = note: /usr/bin/ld: cannot find -lOpenCL
          /usr/bin/ld: cannot find -lOpenCL
          collect2: error: ld returned 1 exit status
```

means that the file libOpenCL.so could not be found in the system's default library paths.
This happens for example when installing CUDA.
To specify an additional folder to search in, use e.g.

```
RUSTFLAGS='-L /usr/local/cuda/lib64/' cargo build --release
```

## History and credits

lisk-vanity is a fork of [nano-vanity](https://github.com/PlasmaPower/nano-vanity) by Lee Bousfield
and Colin LeMahieu. nano-vanity laid the foundation for GPU powered Ed25519 keypair generation.
lisk-vanity added Lisk address derivation and BIP39 passphrase support. The OpenCL implementation
of SHA256/SHA512 is from [hashcat](https://github.com/hashcat/hashcat).
