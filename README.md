# bsp-nuvo-9000

> Standalone board support for the Neousys Nuvo-9000 industrial PC (Intel Core
> i9-14900 / x86-64-v3) with an NVIDIA RTX 4000 SFF Ada discrete GPU.

This extension is **self-contained** — it does not depend on
`avocado-bsp-intel-x86-64-v3`. It bundles the generic Intel x86-64-v3 support
(CPU microcode, networking, WiFi/Bluetooth, audio, USB serial, container
netfilter, common tools) together with the Nuvo-9000-specific hardware:

- **NVIDIA discrete GPU** — open kernel modules, `libcuda`, `nvidia-smi`, GSP firmware
- **5× Intel I226-IT 2.5GbE ports** — the `igc` driver (base BSP ships only e1000e/igb/ixgbe/r8169)
- **Wi-Fi regulatory database** for the AX210
- **Console login on the VT** — the base image enables only `serial-getty@ttyS0`; this adds `getty@tty1`
- **Clean boot** — masks two benign spurious failures (`proc-fs-nfsd.mount`, `getty@getty.service`) and sets `networkd-wait-online --any` (6 NICs)

## Using this extension

`bsp-nuvo-9000` is an [Avocado](https://avocadolinux.org) extension — a reusable fragment of
build- and runtime-configuration that you compose into your own Avocado project. To use it,
declare it as a package-sourced extension in your `avocado.yaml` and add it to a runtime:

```yaml
extensions:
  avocado-bsp-nuvo-9000:
    source:
      type: package
      version: "*"        # or pin an exact version

runtimes:
  my-runtime:
    extensions:
      - avocado-bsp-nuvo-9000
```

Then install and build:

```sh
avocado install   # fetches + installs the SDK, extensions and runtime deps from your config
avocado build     # builds the SDK compile steps, extensions and runtime images
```

`avocado install` pulls the extension from your target's package feed and merges its
config into your project; `avocado build` then produces the runtime.
