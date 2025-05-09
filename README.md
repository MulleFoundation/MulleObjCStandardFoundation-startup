# MulleObjCStandardFoundation-startup

#### ▶️  Startup code for MulleObjCStandardFoundation

This static library provides the required `__register_mulle_objc_universe`
function for executables, that link against
[MulleObjCStandardFoundation](//github.com/MulleFoundation/MulleObjCStandardFoundation).

> Hardly anyone ever does this though, except MulleObjCStandardFoundation tests.

Projects will usually rather link against the
[Foundation](//github.com/MulleFoundation/Foundation) though, and will use
its startup library.


| Release Version                                       | Release Notes  | AI Documentation
|-------------------------------------------------------|----------------|---------------
| ![Mulle kybernetiK tag](https://img.shields.io/github/tag/MulleFoundation/MulleObjCStandardFoundation-startup.svg) [![Build Status](https://github.com/MulleFoundation/MulleObjCStandardFoundation-startup/workflows/CI/badge.svg)](//github.com/MulleFoundation/MulleObjCStandardFoundation-startup/actions) | [RELEASENOTES](RELEASENOTES.md) | [DeepWiki for MulleObjCStandardFoundation-startup](https://deepwiki.com/MulleFoundation/MulleObjCStandardFoundation-startup)





## Info

The main raison d'être of MulleObjCStandardFoundation-startup as a
seperate library is to bequeath the required dependencies
[mulle-atinit](//github.com/mulle-core/mulle-atinit) and
[mulle-atexit](//github.com/mulle-core/mulle-atexit) for linking with the
executable.




## Requirements

|   Requirement         | Release Version  | Description
|-----------------------|------------------|---------------
| [MulleObjCStandardFoundation](https://github.com/MulleFoundation/MulleObjCStandardFoundation) | ![Mulle kybernetiK tag](https://img.shields.io/github/tag/MulleFoundation/MulleObjCStandardFoundation.svg) [![Build Status](https://github.com/MulleFoundation/MulleObjCStandardFoundation/workflows/CI/badge.svg?branch=release)](https://github.com/MulleFoundation/MulleObjCStandardFoundation/actions/workflows/mulle-sde-ci.yml) | 🚤 Objective-C classes based on the C standard library
| [mulle-atinit](https://github.com/mulle-core/mulle-atinit) | ![Mulle kybernetiK tag](https://img.shields.io/github/tag/mulle-core/mulle-atinit.svg) [![Build Status](https://github.com/mulle-core/mulle-atinit/workflows/CI/badge.svg?branch=release)](https://github.com/mulle-core/mulle-atinit/actions/workflows/mulle-sde-ci.yml) | 🤱🏼 Compatibility library for deterministic initializers
| [mulle-atexit](https://github.com/mulle-core/mulle-atexit) | ![Mulle kybernetiK tag](https://img.shields.io/github/tag/mulle-core/mulle-atexit.svg) [![Build Status](https://github.com/mulle-core/mulle-atexit/workflows/CI/badge.svg?branch=release)](https://github.com/mulle-core/mulle-atexit/actions/workflows/mulle-sde-ci.yml) | 👼 Compatibility library to fix atexit

### You are here

![Overview](overview.dot.svg)

## Add

Use [mulle-sde](//github.com/mulle-sde) to add MulleObjCStandardFoundation-startup to your project:

``` sh
mulle-sde add github:MulleFoundation/MulleObjCStandardFoundation-startup
```

## Install

Use [mulle-sde](//github.com/mulle-sde) to build and install MulleObjCStandardFoundation-startup and all dependencies:

``` sh
mulle-sde install --prefix /usr/local \
   https://github.com/MulleFoundation/MulleObjCStandardFoundation-startup/archive/latest.tar.gz
```

### Legacy Installation

Install the requirements:

| Requirements                                 | Description
|----------------------------------------------|-----------------------
| [MulleObjCStandardFoundation](https://github.com/MulleFoundation/MulleObjCStandardFoundation)             | 🚤 Objective-C classes based on the C standard library
| [mulle-atinit](https://github.com/mulle-core/mulle-atinit)             | 🤱🏼 Compatibility library for deterministic initializers
| [mulle-atexit](https://github.com/mulle-core/mulle-atexit)             | 👼 Compatibility library to fix atexit

Download the latest [tar](https://github.com/MulleFoundation/MulleObjCStandardFoundation-startup/archive/refs/tags/latest.tar.gz) or [zip](https://github.com/MulleFoundation/MulleObjCStandardFoundation-startup/archive/refs/tags/latest.zip) archive and unpack it.

Install **MulleObjCStandardFoundation-startup** into `/usr/local` with [cmake](https://cmake.org):

``` sh
PREFIX_DIR="/usr/local"
cmake -B build                               \
      -DMULLE_SDK_PATH="${PREFIX_DIR}"       \
      -DCMAKE_INSTALL_PREFIX="${PREFIX_DIR}" \
      -DCMAKE_PREFIX_PATH="${PREFIX_DIR}"    \
      -DCMAKE_BUILD_TYPE=Release &&
cmake --build build --config Release &&
cmake --install build --config Release
```

## Author

[Nat!](https://mulle-kybernetik.com/weblog) for Mulle kybernetiK  


