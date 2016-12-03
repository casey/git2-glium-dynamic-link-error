god i hate dynamic linking
==========================

Trying to use `openssl-sys` and `core-graphics` at the same time causes a dynamic linking load error at run time:

```
dyld: Symbol not found: _iconv
  Referenced from: /System/Library/PrivateFrameworks/LanguageModeling.framework/Versions/A/LanguageModeling
  Expected in: /opt/local/lib/libiconv.2.dylib
 in /System/Library/PrivateFrameworks/LanguageModeling.framework/Versions/A/LanguageModeling
```

I'm on macOS and have macports installed, however I don't have any of the DYLIB path stuff in my env which normally causes such bugs.

I originally hit this error when using `git2-rs` and `glium`, but managed to whittle it down to just `core-graphics` and `openssl-sys`. These two crates have dependencies, but depending on their transitive dependencies directly isn't enough to trigger the error, so it must be caused by them and not their dependencies.

I think, although I'm not certain, that something in the openssl-sys build script messes up dynamic loading for core-graphics.
