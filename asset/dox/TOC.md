# MulleObjCStandardFoundation-startup Library Documentation for AI

## 1. Introduction & Purpose

**MulleObjCStandardFoundation-startup** provides initialization and startup hooks for the MulleObjCStandardFoundation framework. It handles framework initialization, resource loading, and startup sequences needed for proper Foundation operation.

This library is particularly useful for:
- Framework initialization
- Startup sequence management
- Resource loading at startup
- Framework lifecycle management

## 2. Key Concepts & Design Philosophy

- **Initialization**: Centralized startup handling
- **Hooks**: Extensible initialization hooks
- **Lazy Loading**: Defer initialization until needed
- **Cleanup**: Proper shutdown sequences

## 3. Core API & Data Structures

### Startup Functions

- `void MulleObjCStandardFoundation_startup()`
  - Initialize MulleObjCStandardFoundation framework
  - Call once at application startup

- `void MulleObjCStandardFoundation_shutdown()`
  - Shutdown framework and clean resources
  - Call once at application termination

### Initialization Phases

- `void MulleObjCStandardFoundation_phase_1()`
  - Phase 1: Core class registration

- `void MulleObjCStandardFoundation_phase_2()`
  - Phase 2: Category loading

- `void MulleObjCStandardFoundation_phase_3()`
  - Phase 3: Final initialization

## 4. Integration Examples

### Example 1: Framework Startup
```objc
@interface MyApplication : NSObject
@end

@implementation MyApplication
+ (void) initialize {
    [super initialize];
    MulleObjCStandardFoundation_startup();
}
@end
```

### Example 2: Main Function
```c
int main(int argc, char *argv[]) {
    MulleObjCStandardFoundation_startup();
    
    @autoreleasepool {
        // Application code
    }
    
    MulleObjCStandardFoundation_shutdown();
    return 0;
}
```

## 5. Dependencies

- **MulleObjCStandardFoundation** - Core framework
- **mulle-objc** (runtime)

## 6. Version Information

MulleObjCStandardFoundation-startup version macro: `MULLE_OBJC_STANDARD_FOUNDATION_STARTUP_VERSION`
