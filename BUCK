cxx_binary(
    name = "hello",
    srcs = ["main.cpp"],
    # This syntax just means that this target is only compatible with C++23
    # and C++26.
    # See https://github.com/facebook/buck2/issues/713#issuecomment-2218651714
    # for more information.
    target_compatible_with = select({
        "toolchains//:cxx23": [],
        "toolchains//:cxx26": [],
    }),
)
