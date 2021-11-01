## Lossy Data Compression with SPERR

SPERR performs lossy compression on 2D or 3D scientific (floating-point) data.
Its output can be either _size-bounded_ (expressed as bit-per-pixel, or BPP) or
_point-wise-error-bounded (PWE-bounded)_.

Under the hood, SPERR uses wavelet transforms, [SPECK](https://ieeexplore.ieee.org/document/1347192) coding,
and a custom outlier coding algorithm to achieve efficient compression.
The name of SPERR stands for **SP**eck with **ERR**or bounding.

## Table of Contents

1. [High-Level Overview](#high-level-overview)
2. [Build From Source](#build-from-source)
3. [Command Line Utilities](#command-line-utilities)
4. Programming API

## High-Level Overview <a name="high-level-overview"></a>

SPERR is designed to perform lossy compression on 2D and 3D floating-point
data, which is often produced by numerical simulations.
Being a lossy compressor means that SPERR will not produce
bit-for-bit reconstructions.
Instead, (almost) every data point in the
reconstruction will contain a small error.
The tradeoff is that the more errors are allowed,
the more compression SPERR can achieve.

In its _size-bounded_ compression mode, SPERR takes in a `BPP` value that
controls the level of compression. A valid BPP value is in the range of
(0.0, 32.0) for single-precision floating-point values, and (0.0, 64.0) for
double-precision floating-point values.
Often, a BPP value of 4.0 to 8.0 yields accurate enough reconstructions for
the subsequent analysis.

In its _PWE-bounded_ compression mode, SPERR takes in two values that control
the level of compression: the desired PWE error tolerance `Tol` and a
quantization level `q`.
`Tol` must be positive and is often two to four orders of magnitude smaller
than the input data range.
Given a particular data set and a `Tol`, a proper `q` is required for SPERR
to achieve the most compression.
The mechanisms of `q` and how to choose a proper value for it is discussed
on [this page](https://github.com/shaomeng/SPERR/wiki/SPERR-parameter:-quantization-level-q).


## Build From Source <a name="build-from-source"></a>

SPERR is written in C++, and uses CMake configuration system.
To successfully compile SPERR from source, the build system needs to have
1. a C++ compiler that supports C++ 17 standard;
2. a C++ compiler that supports OpenMP; and
3. CMake.

The configuration and compilation steps are the same with any CMake-based
project.
Note that one can simply configure and compile the code; if any of the
requirements isn't met, either the configuration of compilation step will fail.
More details about building SPERR from source, including build options,
are documented on [this page](https://github.com/shaomeng/SPERR/wiki/Build-SPERR-From-Source).


## Command Line Utilities <a name="command-line-utilities"></a>

Upon a successful build, the following command line utilities are generated in the `bin` directory.
Each utility program contains a set of parameters, and the best way to reference
each parameter is to look at the help page of each utility program, e.g.,
`compressor_3d -h` or `compressor_3d --help`.
Here is a list of all command line utilities that are built by default.

- `compressor_3d`: it takes in a 3D volume and produces compressed bitstream.
  A set of options that describes the input volume and compression parameters
  is required.
- `decompressor_3d`: it takes in a SPERR compressed bitstream and produces a
  reconstruction 3D volume.
  Note that a `decompressor_3d` compiled in size-bounded mode can only decompress
  a bitstream produced by size-bounded `compressor_3d`.
  The same is also true for PWE-bounded `compressor_3d` and `decompressor_3d`.
- `probe_3d`: it provides interactive actions for users to explore the tradeoff
  between compression levels and qualities, and locate the best compression parameters.
  Compression levels are expressed as average bit-per-pixel (BPP), and
  compression qualities are expressed using peak signal-to-noise ratio (PSNR),
  maximum point-wise error (PWE), root-mean-square error (RMSE).
- `compressor_2d`, `decompressor_2d`, and `probe_2d` are currently under development.

An example demonstrating the usage of `probe_3d` is provided on
[this page](https://github.com/shaomeng/SPERR/wiki/Exploring-compression-parameters-using-probe_3d).
