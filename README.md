# canopen-dictator-example

The reference setup for using [Dictator](https://github.com/golovinengineer/canopen-dictator)
from CMake in CI: a minimal host "firmware" whose CANopen object dictionary
is generated from `device.objdict.toml` at build time, for the
[co_stack](https://github.com/RoboticsHardwareSolutions/co_stack) CanFestival
stack.

[`.github/workflows/build.yml`](.github/workflows/build.yml) installs Dictator
with its GitHub Action, runs `dictator check device.objdict.toml --strict`,
builds the firmware and uploads the generated EDS and Markdown. A second job
checks that `dictator check` fails on [`broken/`](broken/device.objdict.toml).
It also runs weekly and on a `dictator-release` repository dispatch.

To copy it, take `CMakeLists.txt` and the `build` job, and put your own
project file and sources in place of `device.objdict.toml` and `main.c`.
`main.c` is only compiled, not linked: the object dictionary refers to the
stack's functions, which a real firmware builds and this example doesn't.
See [Using with CMake](https://github.com/golovinengineer/canopen-dictator#using-with-cmake)
for the options.

Locally, with `dictator` installed:

```sh
dictator cmake install
cmake -S . -B build
cmake --build build
cmake --build build --target od_docs   # build/device.eds, build/device.md
```

## Licence

Licensed under either of [Apache License 2.0](LICENSE-APACHE) or
[MIT](LICENSE-MIT), at your option.
