# rules_js exec-platform optional dependency repro

This repository demonstrates a `rules_js` platform-selection issue for native
optional npm dependencies used by tools, plus a local `rules_js` patch.

`rolldown` loads a native optional package such as
`@rolldown/binding-linux-x64-gnu` at runtime. When Bazel is invoked with a
Linux arm64 target platform on an x64 Linux runner, unpatched `rules_js` hoists
`js_run_binary` tool runfiles from the target configuration and omits the x64
binding, even though the `js_binary` action executes on the x64 execution
platform.

This repo applies `rules_js_exec_tool_runfiles.patch` with a Bzlmod
`single_version_override`. The patch makes `js_run_binary` hoist execroot tool
runfiles through an exec-configured helper rule.

Native target build:

```sh
bazel build //:uses_rolldown
```

Cross-target build that still uses the x64 execution-platform binding:

```sh
bazel build //:uses_rolldown --platforms=//:linux_arm64
```
