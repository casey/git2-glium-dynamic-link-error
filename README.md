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

I then started stripping more things away, and found that just the build script from `openssl-sys` is enough to trigger the error.

Now I have the error triggering just by running `pkg_config::find_library("openssl")` in the build script.

This produces the following output from the build script:

```
cargo:rustc-link-search=native=/opt/local/lib
cargo:rustc-link-lib=ssl
cargo:rustc-link-lib=crypto
```

Although just the first line is enough to trigger the error.

I also removed the dependency on core-graphics-rs, since just the following in main.rs is enough to trigger the error:

```
#[link(name = "ApplicationServices", kind = "framework")]
extern {
}
```
