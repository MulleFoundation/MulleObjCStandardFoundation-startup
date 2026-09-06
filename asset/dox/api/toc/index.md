# MulleObjCStandardFoundation-startup Library Documentation for AI
<!-- Keywords: startup, objective-c, runtime, foundation, universe, stacktrace, atinit -->
## 1. Introduction & Purpose

**MulleObjCStandardFoundation-startup** is a small *static* C/Objective-C
library that must be linked into an executable (or test binary) that links
against `MulleObjCStandardFoundation`. Its sole purpose is to provide the
`bang` startup hook: the function that kicks off the ObjC runtime universe
with the MulleObjCStandardFoundation configuration when the process starts.

- It is *not* a runtime or framework; it contains no classes and exports no
  public API.
- It solves the "who initializes the universe for this foundation variant?"
  problem: the executable's startup needs to know which foundation library
  (usually `MulleFoundation`, sometimes `MulleObjC` or
  `MulleObjCStandardFoundation`) provides the universe configuration.
- Its main raison d'être as a separate library is to bequeath the required
  startup dependencies `mulle-atinit` / `mulle-atexit` to the executable's
  link line.
- The README notes: "Hardly anyone ever does this though, except
  MulleObjCStandardFoundation tests." Most projects link
  [Foundation](https://github.com/MulleFoundation/Foundation) instead, which
  has its own startup library.

## 2. Key Concepts & Design Philosophy

- **The `bang` pattern:** The mulle-objc runtime wiring expects a static
  function of the form `bang( universe, allocator, userinfo)`. This project
  implements exactly that function and lets the runtime's private startup
  include files (`mulle-objc-startup-private.inc`,
  `mulle-foundation-startup-private.inc`) register and invoke it
  automatically at process startup (via the `mulle-atinit` / `mulle-atexit`
  machinery). The application never calls `bang` directly.
- **Configuration defaults:** Instead of hard-coding flags, the foundation
  provides `mulle_foundation_universeconfiguration_set_defaults`, which fills
  a `struct _mulle_objc_universeconfiguration` with the standard
  MulleObjCStandardFoundation settings; the actual class registration is
  delegated to `MulleObjCStandardFoundationBang`.
- **Startup-time side effects:** Since version 0.21.0 the `bang` hook also
  installs a stack-trace callback on the runtime universe
  (`mulle_objc_universe_set_stacktrace_callback( universe,
  mulle_stacktrace_once)`), so that runtime errors report C stack traces via
  `mulle-stacktrace`.
- **No exported public symbols:** The entire API surface is internal to the
  library; the only externally visible artifacts are the "`bang` ran" behavior
  and the linked-in startup machinery. There are no public header files.

## 3. Core API & Data Structures

There are **no public headers** in this project (see the installed-header
installation in `CMakeLists.txt`, which is commented out). The entire source
is a single file, `src/MulleObjCStandardFoundation-startup.m`. The symbols
below are copied verbatim from that file.

### 3.1. `src/MulleObjCStandardFoundation-startup.m`

#### `static void bang(...)` — the startup hook
- **Purpose:** Called automatically by the runtime startup machinery when the
  executable launches. Sets up the universe configuration defaults, runs the
  `MulleObjCStandardFoundation` universe `Bang`, and installs the
  `mulle-stacktrace` callback.
- **Verbatim definition:**

```c
static void   bang( struct _mulle_objc_universe *universe,
                    struct mulle_allocator *allocator,
                    void *userinfo)
{
   struct _mulle_objc_universeconfiguration   config;

   mulle_foundation_universeconfiguration_set_defaults( &config);
   MulleObjCStandardFoundationBang( universe, allocator, &config);
   mulle_objc_universe_set_stacktrace_callback( universe,
                                                 mulle_stacktrace_once);
}
```

- Parameters:
  - `universe` — the `_mulle_objc_universe` being bootstrapped (provided by
    the runtime).
  - `allocator` — the `mulle_allocator` for the bootstrapping phase.
  - `userinfo` — currently unused slot reserved by the startup mechanism.
- The functions it calls (`mulle_foundation_universeconfiguration_set_defaults`,
  `MulleObjCStandardFoundationBang`,
  `mulle_objc_universe_set_stacktrace_callback`, `mulle_stacktrace_once`) come
  from the dependency projects, not from this library.

#### Forward declaration of the stacktrace setter
- Declared in this file (with `MULLE_OBJC_RUNTIME_GLOBAL`) and provided by the
  mulle-objc runtime:

```c
MULLE_OBJC_RUNTIME_GLOBAL
void
   mulle_objc_universe_set_stacktrace_callback(
      struct _mulle_objc_universe *universe,
      void (*callback)( FILE *fp));
```

#### Version macro
- The source-level version constant (value `0.21.0`):

```c
#define MULLE_OBJC_STANDARD_FOUNDATION__STARTUP_VERSION  ((0UL << 20) | (21 << 8) | 0)
```

(Note: the macro name is `MULLE_OBJC_STANDARD_FOUNDATION__STARTUP_VERSION`
— with a *double* underscore after `FOUNDATION`.)

#### Startup-include mechanism (behavior, not an API)
- The file pulls in the private startup include files that wire `bang` into
  the process startup sequence:
  - `<MulleObjCStandardFoundation/MulleObjCStandardFoundation.h>`
  - `<MulleObjCStandardFoundation/mulle-foundation-universeconfiguration-private.h>`
  - `<MulleObjC/MulleObjCExceptionHandler-Private.h>`
  - `<MulleObjC/mulle-objc-startup-private.inc>`
  - `<MulleObjCStandardFoundation/mulle-foundation-startup-private.inc>`
  - `<mulle-stacktrace/mulle-stacktrace.h>`

## 4. Performance Characteristics

- **Not performance-relevant:** The code runs exactly once per process at
  startup, before the main program body executes.
- Startup cost is dominated by the dependency work it triggers
  (universe configuration defaults + class registration via
  `MulleObjCStandardFoundationBang`).
- **Thread-safety:** The `bang` hook runs single-threaded during process
  bootstrap, before any threads exist; no concurrency guarantees are needed.
- **Build constraint:** `CMakeLists.txt` hard-fails with
  `"Startup library must be built static"` if `BUILD_SHARED_LIBS` is set —
  it can only be a static library.

## 5. AI Usage Recommendations & Patterns

- **Best Practices:**
  - Link this library (statically) into executables that link
    `MulleObjCStandardFoundation` — this is what provides the required
    `__register_mulle_objc_universe` startup function and bequeaths
    `mulle-atinit` / `mulle-atexit`.
  - For ordinary applications, prefer linking the `Foundation` project's own
    startup library instead; this one is intended for
    `MulleObjCStandardFoundation`-direct consumers and its tests.
  - Add the dependency via `mulle-sde add
    github:MulleFoundation/MulleObjCStandardFoundation-startup`.
- **Common Pitfalls:**
  - Do **not** attempt to call `bang`, `MulleObjCStandardFoundation_startup`,
    `MulleObjCStandardFoundation_shutdown`, or any "phase" functions — none of
    those are part of this library's API (nor do they exist). The startup runs
    automatically; there is nothing to call from application code.
  - Do not change the `bang` function name/signature or the include order: the
    private `.inc` startup files depend on this exact shape.
  - Do not build it as a shared library (`cmake` will abort).
  - The stacktrace callback is installed unconditionally at startup; if stack
    trace reporting is not desired, that hook must change in this source,
    not at the call site.

## 6. Integration Examples

### Example 1: The canonical startup content (this project's own source)

The whole library, conceptually, is the `bang` hook. This is the idiomatic
shape every `*-startup` library in the mulle-objc ecosystem follows:

```c
// src/MulleObjCStandardFoundation-startup.m (abridged)
#import <MulleObjCStandardFoundation/MulleObjCStandardFoundation.h>
#import <MulleObjCStandardFoundation/mulle-foundation-universeconfiguration-private.h>
#import <MulleObjC/MulleObjCExceptionHandler-Private.h>
#import <MulleObjC/mulle-objc-startup-private.inc>
#include <MulleObjCStandardFoundation/mulle-foundation-startup-private.inc>
#include <mulle-stacktrace/mulle-stacktrace.h>

static void   bang( struct _mulle_objc_universe *universe,
                    struct mulle_allocator *allocator,
                    void *userinfo)
{
   struct _mulle_objc_universeconfiguration   config;

   mulle_foundation_universeconfiguration_set_defaults( &config);
   MulleObjCStandardFoundationBang( universe, allocator, &config);
   mulle_objc_universe_set_stacktrace_callback( universe,
                                                 mulle_stacktrace_once);
}
```

### Example 2: Linking it into an executable with mulle-sde

The startup library is consumed by *being linked*, not by being called.
A consuming project would add it as a dependency:

```sh
mulle-sde add github:MulleFoundation/MulleObjCStandardFoundation-startup
```

The resulting executable then automatically gets the universe bootstrapped
with the MulleObjCStandardFoundation configuration and the
`mulle-stacktrace` callback installed before `main` runs.

## 7. Dependencies

Direct dependencies as listed in `.mulle/etc/sourcetree/config`:

- `MulleObjCStandardFoundation` — marks:
  `no-bequeath,no-header,no-link,no-singlephase` (provides the universe
  configuration defaults and the `MulleObjCStandardFoundationBang` entry
  point).
- `mulle-stacktrace` — header-only dependency, marks:
  `no-all-load,no-cmake-loader,no-cmake-searchpath,no-import,no-link`
  (provides `mulle_stacktrace_once`).

Bequeathed to the executable for linking (see README):

- `mulle-atinit`
- `mulle-atexit`