# Changelog

All notable changes to avocado-bsp-nuvo-9000 are documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.1]

### Added
- 2026 feed support: CI and release legs for `intel-x86-64-v3` on 2026/edge
  (2026/next does not declare that machine).

### Fixed
- Onboard audio on the 6.18 kernel: it splits the Realtek HDA codec per family,
  so 2026 installs `kernel-module-snd-hda-codec-alc882` (the board's ALC888-VD);
  2024 (6.6) keeps `kernel-module-snd-hda-codec-realtek`. This was the only
  package that failed to resolve on 2026; verified on hardware.

## [0.1.0]

### Added
- Initial release: standalone board support for the Neousys Nuvo-9000 industrial
  PC (Intel i9-14900 / x86-64-v3 + a discrete NVIDIA RTX 40-/50-series GPU).
  Self-contained; does not depend on avocado-bsp-intel-x86-64-v3.
- NVIDIA discrete-GPU driver stack, Intel I226 2.5GbE (igc), Wi-Fi regulatory
  database, VT console getty, and masks for benign spurious boot failures plus a
  `networkd-wait-online --any` drop-in for the 6-NIC layout.
- CI via the shared `avocado-linux/actions` reusable workflows: PR build check
  (`test.yml`) and tag-driven package + publish to the Avocado feed (`release.yml`).
