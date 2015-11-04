[TOC]

# Overview

The ``wire-cell`` software in [BNL IF GitHub](https://github.com/BNLIF) depends on some software that may not come with your OS, in particular [ROOT](http://root.cern.ch) v6 and a compiler supports C++11.

There are three likely ways to supply the required externals described below.

1. manually build or otheriwse provide them yourself.

2. automatically build and install them with the [Worch](https://github.com/brettviren/worch)-based build orchestration system.

3. use an existing site-installation.

4. OS-level installation.

The following sections gives details on each of these approaches.


# Manual installation of externals

You may provide Wire Cell externals as you wish.  This section gives
guidance on how to do this.  It mostly does this by references details
in the configuration files that drive the Worch automated build
orchestration described in the next section.

Some general things to assure:

* use the same compiler for all C++ packages.
* use a compiler that supports C++11.
* all C++ packages built against the C++11 standard, eg GCC's `-std=c++11` flag.

Required packages and their recomended versions are in [defaults.cfg](https://github.com/BNLIF/wire-cell-externals/blob/master/defaults.cfg).

The rest of this section gives some guidance on specific packages.

## Compiler

You must use a compiler that supports C++11.
Clang may work but is untested (please report successes/failures).
GCC 4.9.2 is what the developers currently use.

Building GCC is relatively straight-forward but it does involve a
half-dozen packages including GCC itself.

No special tricks are needed.  See [compiler.cfg](https://github.com/BNLIF/wire-cell-externals/blob/master/compiler.cfg) for detailed URLs and build flags.

## ROOT6

Details of how Worch builds ROOT are in the [packages.cfg](https://github.com/BNLIF/wire-cell-externals/blob/master/packages.cfg) file.  CMake is used and the flag `-Dcxx11=ON` assures C++11 is used.  Other CMake options must be passed to tell it where to find the other externals (eg, Python, FFTW).  See the `packages.cfg` file for details.

You may wish to turn on additional features.  Wire Cell needs:

* minuit2
* python
* fftw

# Automated installation with Worch

This method will automatically build the necessary packages from
source.  It requires some initial manual preparation followed by
unattended, if lengthy, building.

## Quickstart

Before you start, make sure you have the essitial build tools from the OS.
For Example, in *Ubuntu*, you need to have the following packages installed:

```bash
sudo apt-get install git build-essential libboost-all-dev libsqlite3-dev tcl8.5-dev libxpm-dev libxft-dev libXext-dev
```

If you don’t care what this does, just cut and paste.  If something "doesn't work" then read the subsequent sections.

```bash
$ virtualenv /path/to/buildenv
$ source /path/to/buildenv/bin/activate
$ pip install worch

$ git clone git@github.com:BNLIF/wire-cell-externals.git /path/to/work
$ cd /path/to/work
$ waf --prefix=/path/to/install --orch-config=worch.cfg configure build
$ deactivate

$ source /path/to/install/env.sh
$ module load root
```

You would repeat those last two lines in any new shells. You can now test:

```bash
$ root -b -q
...
| Welcome to ROOT 6.02/05                http://root.cern.ch |
...
$ python -c 'import ROOT; print ROOT.gROOT.GetVersion()'
6.02/05
```

More details and build options are described in the following sections.

## Preparing Worch

### Install Worch in Virtualenv

It is recommended to install the Worch build orchestration system into a Python virtual environment. This environment will be used only for building these externals.

```bash
$ virtualenv /path/to/buildenv
```

If you system does not already provide it, [download virtualenv](https://virtualenv.pypa.io/en/latest/installation.html) and unpack it.  From the unpacked directory you can simply run

```bash
$ python virtualenv.py /path/to/buildenv
```

In either case one activates the virtual environment and installs worch like:

```bash
$ source /path/to/buildenv/bin/activate
$ pip install worch
```

### Install Worch in user dir

Optionally, if you already have `pip` available you may avoid using `virtualenv` and install Worch into your "user" area under `~/.local/`:

```bash
$ pip install --user worch
```

You will need to assure `$HOME/.local/bin` is in your `PATH`.

### Wire-Cell Worch Configuration

However you end up supplying Worch, the `wire-cell-externals` repository provides the rest of what is needed. Clone it to some place with ample disk space (10s of GB).

```bash
$ git clone git@github.com:BNLIF/wire-cell-externals.git /path/to/work
$ cd /path/to/work
$ waf --help
```

## Build

After the above is done, this one command builds all the external software:

```bash
(1)  $ ls worch-*.cfg
(2)  $ waf --prefix=/path/to/install --orch-config=worch-<config>.cfg configure
(3)  $ waf
(4)  $ deactivate
```
This does:

1. See which main configuration file is suited to your environment.
2. Configures the build according to the ``worch-<config>.cfg`` file and tells it where to install the final results.
3. Performs the automated build of external packages. This takes ~1 hour on Intel i7 w/ an SSD.
4. Exit the virtualenv as it is no longer needed (``/path/to/buildenv`` may be removed)

## Install

There are two ways to install the results of the build which differ in the organization of the installation directory layout. They are:

- **name/version tree:** Each package is installed into ``/path/to/install/<name>/<version>/{bin,lib,include}``. This allows for multiple versions of each package to be installed in parallel supporting different versions of an overall collection. It allows purging of a particular version by simply deleting the directory, although with care not to break other packages which depend on it. The user environment must be modified (``PATH``, etc) to pick up all run-time aspects of each individual package.

- **single-rooted directory:** All packages are installed in the same ``/path/to/install/{bin,lib,include}`` directory. User run-time environment still requires adjustment but only for a single aspect. Only one version of a package may be installed, and removing an installed package is not supported (but is possible).
This package supports both paradigms.

This package supports both paradigms.

### Install to name/version tree

By default, and as part of the build procedure, one will produce a name/version tree installation area as designated by the ``--prefix`` option to ``waf``. The user run-time environment can be set up with the help of [Environment Modules](http://modules.sf.net/) (EM) which are provided by the build.

```bash
$ source /path/to/install/env.sh
$ module load root

$ root -b -q
...
| Welcome to ROOT 6.02/05                http://root.cern.ch |
...

$ python -c 'import ROOT; print ROOT.gROOT.GetVersion()'
6.02/05
```

### Single-rooted install

In addition to the population of `/path/to/install` as above, the build will produce a "tarpack" binary package file. These can simply be unpacked into a single-rooted directory.

```bash
$ cd /path/to/work
$ mkdir -p /path/to/single-rooted
$ for n in tmp/tarpack/*.tgz; do tar -C /path/to/single-rooted -xf $n; done
```
No special environment setup mechanism is provided for this mechanism as one can largely piggy-back on the one ROOT provides:

```bash
$ source /path/to/single-rooted/bin/thisroot.sh

$ root -b -q
...
| Welcome to ROOT 6.02/05                http://root.cern.ch |
...

$ python -c 'import ROOT; print ROOT.gROOT.GetVersion()'
6.02/05
```

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
