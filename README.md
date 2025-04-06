# Sample Buck2 project

TODO: Write some docs for this.

Showcase how to:

- Do custom toolchains
- Define build configurations
- Use modifiers, including aliases
- Use PACKAGE files maybe?

```sh
# Yay!
buck2 run :hello -m toolchains//:cxx23
buck2 run :hello -m toolchains//:cxx26
# Boo!
buck2 run :hello -m toolchains//:cxx20
```
