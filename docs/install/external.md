[TOC]

# Overview

The ``wire-cell`` software in [BNL IF GitHub](https://github.com/BNLIF) depends on some software that may not come with your OS, in particular [ROOT](http://root.cern.ch) v6 and a possibly a compiler that supports C++11.

There are a few ways ways to supply the required externals described below.

1. manually build or otherwise provide them yourself.

2. automatically build and install them with [Spack](https://github.com/LLNL/spack) with support provided by the Wire Cell Toolkit's [wire-cell-spack](https://github.com/WireCell/wire-cell-spack) package.

3. use an existing site-installation.

4. OS-level installation.

The following sections gives some guidance on each of these approaches.


# Manual installation of externals

You may provide Wire Cell externals as you wish.  This section gives
guidance on how to do this.

Some general things to assure:

* use the same compiler for all C++ packages.
* use a compiler that supports C++11.
* all C++ packages built against the C++11 standard, eg GCC's `-std=c++11` flag.

The rest of this section gives some guidance on specific packages.

## Compiler

You must use a compiler that supports C++11.
Clang may work but is untested (please report successes/failures).
GCC 4.9.2 is what the developers currently use.  

Newer GCCs are more strict, particularly w.r.t. headers will likely uncover bugs of omission.  Please report them to the developers.

## ROOT6

CMake should used with at least these flags:

```shell
cmake -Dminuit2=ON -Dpython=ON -Dcxx11=ON -DFFTW_LIBRARY=/path/to/lib/libfftw3.so [...]
```

# Automated installation with Spack

[Spack](https://github.com/LLNL/spack) is a "meta build system" that
runs the individual build systems that come with packages.  It allows
one to manage an ever growing installation area which can accommodate
multiple versions of a package.  It also comes with support
for [Environment Modules](http://modules.sourceforge.net/)
 to handle your users' setup of these packages or can make targeted release "views" of its package tree.

The Wire Cell Toolkit provides a package [wire-cell-spack](https://github.com/WireCell/wire-cell-spack)
which collects instructions and a Spack "repo" that builds WCT and its third-party dependencies.  This leverages Spacks built-in "repo" to provide dependencies needed by WCT's direct dependencies.  Using it will tend to build packages that one may already have installed through the OS (eg, Python).  However, this duplication should not add much to the overall build time which is automatic nor lead to any problems.

An installer that wishes to use wire-cell-spack to provide the dependencies should begin by following its [README](https://github.com/WireCell/wire-cell-spack/blob/master/README.org) file.

Although developed for the Wire Cell Toolkit, the resulting external dependencies will be suitable for the prototype.  However, some additional packages not strictly required may be built.

# Site installations

Some sites associated with Wire Cell development have already been
provisioned with the necessary externals and you may leverage them.

## RACF Setup

At BNL's RACF, a simple, single-rooted installation of Wire Cell external packages is provided.

Bash users do:

```bash
$ source /gpfs01/lbne/users/sw/wc/bin/thisroot.sh
```

Or, if you are still stuck using `tcsh` do:

```csh
> source /gpfs01/lbne/users/sw/wc/bin/thisroot.csh
```

## FNAL Setup

At Fermilab, externals are provided by UPS.

```bash
$ source /grid/fermiapp/products/larsoft/setups
$ setup root v6_04_02 -q e7:prof
$ setup boost v1_57_0 -q e7:prof
```

# OS level installation

## Mac

Install homebrew and do

```bash
$ brew install boost
$ brew install homebrew/science/root6
```
