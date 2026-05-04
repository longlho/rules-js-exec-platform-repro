# rules_js exec-platform optional dependency repro

This repository demonstrates a `rules_js` platform-selection issue for native
optional npm dependencies used by tools.

`rolldown` loads a native optional package such as
`@rolldown/binding-linux-x64-gnu` at runtime. When Bazel is invoked with a
Linux arm64 target platform on an x64 Linux runner, `rules_js` filters the
optional dependency using the target platform and omits the x64 binding, even
though the `js_binary` action executes on the x64 execution platform.

Native target build:

```sh
bazel build //:uses_rolldown
```

Cross-target build that should still use the x64 execution-platform binding,
but fails:

```sh
bazel build //:uses_rolldown --platforms=//:linux_arm64
```
