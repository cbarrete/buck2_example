# Sample Buck2 project

TODO: Write some docs for this.

Showcases how to:

- Do custom toolchains
- Define build configurations
- Use modifiers, including aliases
- Use PACKAGE files

```sh
# Yay!
buck2 run :hello -m cxx23
buck2 run :hello -m cxx26
# Boo!
buck2 run :hello -m cxx20
```
