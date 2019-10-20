# MulleObjCStandardFoundation-startup

▶️ Startup library for MulleObjCStandardFoundation

This static library provides the required `__register_mulle_objc_universe`
function for executables, that link against
[MulleObjCStandardFoundation](https://github.com/mulle-objc/MulleObjCStandardFoundation).

> Hardly anyone ever does this though, except MulleObjCStandardFoundation tests.

Projects will usually rather link against the
[Foundation](https://github.com/mulle-objc/Foundation) though, and will use
its startup library.



Build Status | Release Version
-------------|-----------------------------------
[![Build Status](https://travis-ci.org/MulleFoundation/MulleObjCStandardFoundation-startup.svg)](https://travis-ci.org/MulleFoundation/MulleObjCStandardFoundation-startup) | ![Mulle kybernetiK tag](https://img.shields.io/github/tagMulleFoundation/MulleObjCStandardFoundation-startup.svg) [![Build Status](https://travis-ci.org/MulleFoundation/MulleObjCStandardFoundation-startup.svg?branch=release)](https://travis-ci.org/MulleFoundation/MulleObjCStandardFoundation-startup)


## Sourcetree

The main raison d'être of MulleObjCStandardFoundation-startup as a
seperate library is to bequeath the required dependencies
[mulle-atinit](//github.com/mulle-core/mulle-atinit) and
[mulle-atexit](//github.com/mulle-core/mulle-atexit) for linking with the
executable.


## Install

See [mulle-foundation-developer](//github.com/MulleFoundation/mulle-foundation-developer) for
installation instructions.


## Build

### Manually with cmake

Install all above prerequisites:

now build the project

```
(
   mkdir build &&
   cd build &&
   cmake .. &&
   make
)
```

### Conveniently with mulle-sde

Install [mulle-sde](//github.com/mulle-sde) and run `mulle-sde craft`.


### Platforms and Compilers

All platforms and compilers supported by
[mulle-c11](//github.com/mulle-c/mulle-c11/) and
[mulle-thread](//github.com/mulle-concurrent/mulle-thread/).


## Author

[Nat!](//www.mulle-kybernetik.com/weblog) for
[Mulle kybernetiK](//www.mulle-kybernetik.com) and
[Codeon GmbH](//www.codeon.de)
