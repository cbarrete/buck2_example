# Buck2 showcase

This project showcases how to:

- Write/use custom toolchains.
    - In this case, the C++ toolchain supports multiple standard versions.
- Use config modifiers to control the build configuration, including using `PACKAGE` files.

## What's going on?

Here's a quick tour of this repo:

- `toolchains/BUCK` defines a custom C++ toolchain, including the C++ standard version constraint that it reads.
- `BUCK` defines a target for a trivial target that only supports C++23 and C++26.
- `PACKAGE` sets up config modifiers, which are used to set the C++ standard version in all targets in the repo.
- `cxx26_project` is a sub-project that demonstrates how to use `PACKAGE` files to apply config modifiers to a whole directory.

For implementation details, see the comments in the build files.

## How to run

This project is not (currently) hermetic, so you'll need the following installed:

- [`buck2`](https://github.com/facebook/buck2/releases) (this has been tested with `2025-04-01`, but any somewhat recent version should work)
- A Clang toolchain, with the following specifically available:
    - `clang++`
    - `lld`
    - `libstdc++`

    If you're a Nix user, this will do the trick:

    ```nix
    pkgs.mkShell {
      packages = [
        pkgs.clang
        pkgs.lld
      ];
      LD_LIBRARY_PATH = nixpkgs.lib.makeLibraryPath [ pkgs.stdenv.cc.cc ];
    }
    ```

```sh
# Build/run a trivial program in C++26 mode.
buck2 run :hello -m toolchains//:cxx26
# Same thing using an alias defined in the root `PACKAGE` file.
buck2 run :hello -m cxx26
# Try to build it with C++20, which fails at the build system level, because
# this target does not support it (because it uses `std::println`).
buck2 run :hello -m cxx20
# If we do not explicitly set a C++ standard, the target defaults to being
# incompatible.
buck2 run :hello

# Do the same in a project that builds in C++26 by default: no need to specify
# the modifier by hand.
buck2 run cxx26_project:hello
# The default modifiers can still be overridden by hand, so this fails (in the
# compiler, not the build system, because we have not bothered setting the
# compatibility constraints on that target).
buck2 run cxx26_project:hello -m cxx20
```

## TODO

Things that might be worth adding if I have time/feel like it:

- Use `PACKAGE` files beyond config modifiers.
- Add an example for adding a modifier to a target directly (the C++ standard isn't a great example for that).
- Show how build configuration flows throughout the build (i.e. you only have to configure the top-level target and the configuration propagates to dependencies).
- Add a hermetic toolchain, even if it's for something much simpler than C++.
