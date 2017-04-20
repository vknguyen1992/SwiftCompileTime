# Swift Compile Time
How to reduce the compile time of Swift

## Result
- Debug: ~12 minutes → ~4 minutes
- Staging: ~45 minutes → ~23 minu
- Release: N/A

## Steps to reduce building time
### Xcode flag
1. Add User-Defined setting SWIFT_WHOLE_MODULE_OPTIMIZATION = YES (Most affected, reduce compile time from 12 minutes to 4.5 minutes)
Some people say that this flag does not work anymore on Xcode 8.3, I still haven't verify it yet
1. Add User-Defined setting HEADERMAP_USES_VFS = YE1
1. Turn on `Build Active Architecture Only = YES` (Only apply for debug config)
1. Use plain `DWARF` instead of `DWARF with dSYM File` as your `Debug Information Format`
1. Use tool (https://github.com/RobertGummesson/BuildTimeAnalyzer-for-Xcode) to analyze build time of the project (You can manually use terminal to list out all the compile time, but I prefer using tools since it is more visualization)
 >- Download and Archive the project BuildTimeAnalyzer
 >- Added the `-Xfrontend -debug-time-function-bodies` flag to my `Other Swift Flags` in main target's build settings
 >- Build the project
6. Move some utilities, stand-alone codes to a separate local pod to reduce the number of compiled sources in main project (I've reduced from 647 files to 585, yet the compile time is not reduced much (sad))
1. Add `-Xfrontend -warn-long-function-bodies=100` flag to `Other Swift Flags` to get warnings from functions that take longer 100 ms to compiles

### Code Convention
1. Create dictionary
1. Concatenate string
1. Concatenate array
1. Lazy properties

## References
- http://stackoverflow.com/questions/39547197/xcode-8-0-swift-3-0-slow-indexing-and-building
- http://stackoverflow.com/questions/39456223/xcode-8-does-full-project-rebuild
- https://labs.spotify.com/2013/11/04/shaving-off-time-from-the-ios-edit-build-test-cycle/
- http://roadfiresoftware.com/2017/01/improving-swift-compile-times/
- http://stackoverflow.com/questions/25537614/why-is-swift-compile-time-so-slow
- https://medium.com/@RobertGummesson/regarding-swift-build-time-optimizations-fc92cdd91e31
- https://medium.com/swift-programming/swift-build-time-optimizations-part-2-37b0a7514cbe
- http://khanlou.com/2016/12/guarding-against-long-compiles/
- https://github.com/bitrise-io/steps-xcode-test/issues/45
- https://forums.developer.apple.com/thread/62737
- https://lists.swift.org/pipermail/swift-dev/Week-of-Mon-20160411/001693.html
