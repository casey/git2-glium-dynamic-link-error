god i hate dynamic linking
==========================

For some reason `git2-rs` and `glium` aren't playing nice when used together. This is the minimal example I could find that triggers the error. Unfortunately, it might not really be minimal, because it might not be a conflict between git2 and glium, but a conflict between their dependencies.

I'm using macOS and have macports installed, however I don't have any of the DYLIB path stuff in my env which normally causes such bugs.

I'm using glium from git. I tried using git2-rs from git, but it didn't build on my system.

The output from cargo run:

```
$ cargo run
cargo run
   Compiling git2-rs-linker-error v0.0.0 (file:///Users/rodarmor/src/git2-glium-dynamic-link-error)
    Finished debug [unoptimized + debuginfo] target(s) in 0.18 secs
     Running `target/debug/git2-rs-linker-error`
dyld: Symbol not found: _iconv
  Referenced from: /System/Library/PrivateFrameworks/LanguageModeling.framework/Versions/A/LanguageModeling
  Expected in: /opt/local/lib/libiconv.2.dylib
 in /System/Library/PrivateFrameworks/LanguageModeling.framework/Versions/A/LanguageModeling
error: Recipe `default` failed on line 2 with exit code 1
```
