# UniXNU-sm8250-common

Shared Qualcomm SM8250 / SM8250-AC (kona) support for [UniXNU](https://github.com/UniXNU/UniXNU):
CPU topology, GICv3, timer rate, controller register blocks, the kernel config fragment and
the SoC XNU patch series. Device repositories select it through a pinned `kind = "soc"` source
and supply memory layout, enabled peripherals, components, firmware, power policy and packaging.

| Path | Contents |
| --- | --- |
| `soc.toml` | SoC name, hardware source and component selection |
| `dts/sm8250.dtsi` | CPUs with E/P cluster types, PSCI, GICv3, GCC, EPSS cpufreq, RSC, SPMI, TSENS, TLMM, QUPv3, UFS, PCIe0, SMMUs, Adreno 650 + GMU, MDSS/DPU, ADSP, USB |
| `configs/sm8250.config` | Kernel config fragment, applied before the device defconfig |
| `patches/xnu/series` | SoC XNU patches in order (empty in this revision) |
| `patches/xnu-12377/series` | SoC patches for xnu-12377.121.6 (the iOS/iPadOS 26 and 27 profiles): the board power hook the OS profile's boot-progress patch applies on, and MPIDR Affinity1 core numbering. Both are UniXNU-sm8550-common's (BSD-2-Clause-Patent) |
| `kits/` | Shared platform operations (empty in this revision) |

## Sources

Register values come from the [HakiOS](https://github.com/oosexgang/hackios-xnu-kernel) drivers,
which drive this SoC on a Xiaomi Mi 10T (apollo) under iOS 16.5. They are cross-checked against
`arch/arm64/boot/dts/qcom/sm8250.dtsi` in Linux v6.12. Each DTSI node names the driver that
uses it. The node layout follows the UniXNU convention set by
[FFT-infra/UniXNU-sm8250-common](https://github.com/FFT-infra/UniXNU-sm8250-common)
(BSD-2-Clause-Patent).

## Planned

The SoC-level HakiOS drivers are still in [UniXNU-apollo](https://github.com/Pistachio-Project/UniXNU-apollo)
under `patches/xnu/`. They move here or into their own component repositories one at a time,
each with the fixed addresses replaced by values read from this DTSI:

| HakiOS driver | Destination |
| --- | --- |
| `hakios.apollo_cpufreq.c` (EPSS cpufreq-hw) | `kits/` |
| `hakios.apollo_adreno.c`, `hakios.apollo_gpu.cpp` (Adreno 650) | `Adreno650` component |
| `hakios.apollo_adsp.c` (LPASS loader, APR) | `kits/` |
| `hakios.apollo_pcie.c` (PCIe0, QMP PHY, MHI) | `kits/` |
| `hakios.apollo_qrtr.c` (QRTR over MHI) | `kits/` |
| SMP numbering and E/P clusters from HakiOS's kernel patch | `patches/xnu/` |

Board parts stay in each device repository: touch, panel, amplifiers, haptics, microphones,
battery and button wiring.

## Verified devices

| Device | Repository | Result |
| --- | --- | --- |
| Xiaomi Mi 10T (apollo) | [UniXNU-apollo](https://github.com/Pistachio-Project/UniXNU-apollo) | HakiOS: iOS 16.5 home screen. UniXNU build: pending |

## Read

[SoC support](https://unixnu.com/porting/soc-support), [device tree](https://unixnu.com/porting/device-tree),
[component catalog](https://unixnu.com/reference/components), [building](https://unixnu.com/building).

Licensed under BSD-2-Clause-Patent (`LICENSE`).
