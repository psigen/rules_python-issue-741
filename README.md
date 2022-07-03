# rules_python - [Issue #741](https://github.com/bazelbuild/rules_python/issues/741)

The output filename of a `py_wheel` target does not respect stamp info rendering
of the `version` field, even though it is used in the internal wheel files.

```
$ bazel build //:wheel

Starting local Bazel server and connecting to it...
INFO: Analyzed target //:wheel (42 packages loaded, 319 targets configured).
INFO: Found 1 target...
Target //:wheel up-to-date:
  bazel-bin/test_wheel-0.0.1_BUILD_TIMESTAMP_-py3-none-any.whl
INFO: Elapsed time: 3.387s, Critical Path: 0.14s
INFO: 7 processes: 6 internal, 1 linux-sandbox.
INFO: Build completed successfully, 7 total actions
```

Note the output file is named `test_wheel-0.0.1_BUILD_TIMESTAMP_-py3-none-any.whl`
instead of rendering the stamp value of `BUILD_TIMESTAMP`.

However, if the `.whl` file is opened, it contains the expected name of e.g.
`test_wheel-0.0.1_1656829011.dist-info` for the `dist-info` directory.

In addition, `bazel-bin/wheel.name` contains the correctly rendered name, e.g.
`test-wheel-0.0.1-1656829011-py3-none-any.whl`, which does not match the output file.
