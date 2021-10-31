## SPERR: Lossy Scientific Data Compression

SPERR performs lossy compression on 2D or 3D scientific (floating-point) data.
Its output can be either _size-bounded_ (expressed as bit-per-pixel, or BPP) or
_point-wise-error-bounded (PWE-bounded)_.

Under the hood, SPERR uses wavelet transforms, [SPECK](https://ieeexplore.ieee.org/document/1347192) coding,
and a custom outlier coding algorithm to achieve efficient compression.
The name of SPERR stands for **SP**eck with **ERR**or bounding.

## Table of Contents

1. [High-level overview](#high-level-overview)
2. [Build from source](#build-from-source)
3. Command line utilities
4. Programming API

## High-level Overview <a name="high-level-overview"></a>

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
are documented [on this page](https://github.com/shaomeng/SPERR/wiki/Build-SPERR-From-Source).




```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/shaomeng/SPERR/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
