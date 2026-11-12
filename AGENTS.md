# Agent instructions

This repository owns shared SM8250 resources and platform operations.
Keep board peripherals, firmware selection, partition layout and power
policy in the device repository. Preserve runtime source filenames,
classes, symbols, bundle IDs and protocol IDs.

Read these UniXNU pages before editing:

1. [Porting](https://unixnu.com/porting/)
2. [SoC support](https://unixnu.com/porting/soc-support)
3. [Device repositories](https://unixnu.com/porting/device-repository)
4. [Components](https://unixnu.com/reference/components)
5. [Device trees](https://unixnu.com/porting/device-tree)

Every DTSI value needs a source: a HakiOS driver that drives it on hardware,
or the Linux revision in `soc.toml`. Name the source in a comment. Do not add
board memory, reserved regions or enabled peripherals here.

Kernel patches target the OS profile the consuming devices select (iOS 16.5,
xnu-8796.121.2, for apollo). State the XNU version each patch was checked on.

Run builds from the UniXNU checkout, with this checkout as a local override:

```sh
./unixnu defconfig apollo_defconfig -D ../UniXNU-apollo --local-module soc:sm8250=../UniXNU-sm8250-common
./unixnu status --json -D ../UniXNU-apollo
```

Report commands, results, required local inputs and hardware checkpoints.
Use plain English describing current behavior.
