# ry-verify

**Version 7.191.0** · [Changelog](CHANGELOG.md)

Standalone audit of the GTR9 Pro CachyOS profile that [ry-install](https://github.com/ryanmusante/ry-install) deploys. `ry-verify.fish` regenerates all 17 [Managed Files](#managed-files) in memory and compares the installed bytes against its own embedded baseline, then reads the live kernel-cmdline, module, sysctl, unit, fstab, and session state — `--verify` reports every check, `--check` probes silently for drift.

## Quick Start

> [!WARNING]
> Run as your normal user — never run the script itself with `sudo`. Meet [Requirements](#requirements) first. Verify after a reboot.

```fish
git clone https://github.com/ryanmusante/ry-verify.git
cd ry-verify
sudo -v
./ry-verify.fish
```

A run closes with `VERIFICATION SUMMARY` and a `Results:` line counting `OK`, `WARN`, `FAIL`, and `GEN_FAIL` — see [Exit Codes](#exit-codes).

## Requirements

`ry-verify.fish` runs no dependency phase — it refuses to start without GNU `id`, `find`, `stat`, `date`, `mktemp`, and `mv -T`, and guards the rest at their call sites.

| Requirement | Detail |
|---|---|
| OS | CachyOS (Arch-based), systemd-boot with BLS entries |
| Shell | fish 3.6 or newer |
| Hardware | CPU matching `Ryzen AI Max` — bypass via [Environment Overrides](#environment-overrides) |
| BIOS | flat 85 W ceiling, `TjMax = 90 °C` — see [BIOS](#bios) |
| Privileges | normal user with sudo rights; `sudo -v` cached before the run |
| Tools | GNU coreutils, `findmnt`, `awk`, `grep`, `find`, `pacman`, `systemctl` |

## Usage

The bare invocation equals `--verify`; `--check` is the silent idempotency probe (the two are mutually exclusive). `--install-file` belongs to [ry-install](https://github.com/ryanmusante/ry-install) and is an unknown option here, exit `2`; the unattended install is that repo's bare invocation. Positional arguments exit `2`. `--help` (`-h`) and `--version` (`-v`) are the only stdout output — every result goes to stderr.

Each run writes one JSONL log (`0600`) to `~/ry-install/logs/YYYY-MM-DD/MODE-YYYYMMDD-HHMMSS±ZZZZ-PID.jsonl`. A fresh install's `./ry-verify.fish --check` reports drift until reboot.

## Exit Codes

`ry-verify.fish` exits `0 1 2 3 10`.

| Code | Meaning |
|---|---|
| `0` | OK — success, `WARN`-only runs, and a clean `--check` |
| `1` | a `--verify` mismatch |
| `2` | bad arguments, root misuse |
| `3` | missing dependency, uncached sudo, gate mismatch; root `--check` is silent |
| `10` | drift — `--check` found drift from the managed baseline |

## Environment Overrides

Skipping the hardware check is the risky override — a wrong-CPU run compares against an incorrect kernel cmdline and initramfs `MODULES`.

| Variable | Effect |
|---|---|
| `RY_INSTALL_SKIP_HARDWARE_CHECK=1` | bypass the `EXPECTED_CPU_MATCH` hard-fail |
| `NO_COLOR` | disable colored output when set to a non-empty value ([no-color.org](https://no-color.org)) |

## Managed Files

Listed in deploy order, audited byte for byte against the regenerated baseline; system files are checked against `0644` where the filesystem records modes, user files against `0600`.

### Boot

| File | Expected content |
|---|---|
| `/boot/loader/loader.conf` | systemd-boot: `default @saved`, `timeout 0`, `console-mode keep`, `editor no` |
| `/etc/kernel/cmdline` | `rw root=UUID=<detected>` plus the 14 kernel tokens |
| `/etc/sdboot-manage.conf` | `LINUX_OPTIONS` mirror, `LINUX_FALLBACK_OPTIONS="quiet"`, entry management keys |
| `/etc/mkinitcpio.conf` | `MODULES` (`amdgpu`, early KMS), `HOOKS`, `COMPRESSION` `zstd` (`-3`) |

### System

| File | Expected content |
|---|---|
| `/etc/systemd/resolved.conf.d/99-cachyos-resolved.conf` | mDNS and LLMNR off |
| `/etc/systemd/logind.conf.d/99-cachyos-logind.conf` | 8 power, suspend, hibernate, and reboot keys ignored, long-press included |
| `/etc/systemd/system/NetworkManager-dispatcher.service.d/logging.conf` | `LogLevelMax=notice` |
| `/etc/NetworkManager/conf.d/99-cachyos-nm.conf` | `wpa_supplicant` backend, Wi-Fi powersave off, unlimited autoconnect retries, log `WARN` |
| `/etc/iw-regdomain` | regulatory domain (`US`) |
| `/etc/bluetooth/main.conf` | auto-power-on, `FastConnectable`, 3 reconnect attempts |
| `/etc/nftables.conf` | default-deny-inbound, IPv4 ping allowed, ICMPv6 base accept |
| `/etc/default/cpupower-service.conf` | governor (`performance`) |
| `/etc/sysctl.d/95-ry-overrides.conf` | `fq` qdisc, TCP `bbr`, VM tunables |
| `/etc/udev/rules.d/99-ry-perf.rules` | NVMe scheduler `none`, P-State EPP, GPU DPM level `high` |
| `/etc/modprobe.d/60-ry-modules.conf` | optional `amdxdna` blacklist — comment-only while `BLACKLIST_AMDXDNA=false` |

### User

| File | Expected content |
|---|---|
| `~/.config/environment.d/10-environment.conf` | session env — DXVK, GTK, MangoHud, Proton, VKD3D, Wine, PowerDevil |
| `~/.config/MangoHud/MangoHud.conf` | readout-only HUD — horizontal, top-left, toggle `Shift_R+F12` |

## Checks

`--verify` runs the static groups first, then the runtime groups. `--check` runs the silent subset and reports drift only; it records unmanaged `60-ry-*` drop-ins before the sudo gate, and orphan masks after it.

| Group | Scope |
|---|---|
| Static: boot | `loader.conf`, `sdboot-manage.conf`, `sdboot-manage.conf.d` drop-ins that outrank it, `/etc/kernel/cmdline` (`KERNEL_PARAMS`, `root=UUID`, `rw`), `mkinitcpio.conf`, `$BOOT` entries |
| Static: system | resolved, logind, NetworkManager dispatcher logging, NetworkManager, `iw-regdomain`, bluetooth, `cpupower-service.conf`, sysctl drop-in, udev, modprobe plus the unmanaged `60-ry-*` sweep, nftables |
| Static: user | `environment.d` (`ENV_VARS`), MangoHud |
| Static: packages | `PKGS_ADD` and `EXPECTED_VULKAN_PKGS` present, `PKGS_DEL` absent, `pacman.conf` `IgnorePkg` and `ParallelDownloads` |
| Static: services | `MASK` unit state, plus masked units the profile no longer declares |
| Static: syntax | live `mkinitcpio.conf` `HOOKS` presence — ordering is not re-checked here |
| Static: checksum | SHA256 of installed bytes against generator output per destination, root-UUID fallback compare, `.ry.bak` recovery copies in `~/ry-install/backups/` non-empty |
| Runtime: kernel | live `/proc/cmdline`, kernel parser rejections, GPU DPM level, CPU governor, EPP, `EXPECTED_SCALING_DRIVER` and boost, module parameters, NVMe I/O scheduler, blacklists |
| Runtime: services | `conf.d`-implied and `EXPECTED_SERVICES` units, `MASK` units inactive, user-scope units, Wi-Fi and NM backend |
| Runtime: environment | session `ENV_VARS`, live sysctl via `/proc/sys`, fstab ext4 entries, live ext4 mount options, `/dev/ntsync`, wireless regulatory domain |
| Runtime: session | NetworkManager system-connections perms, installed file modes, parent directories of managed files |

## Safety and Reliability

**Verification** — `--verify` compares installed bytes to generator output byte for byte, logging both SHA256 digests on a mismatch, then checks live kernel-cmdline, module, sysctl, unit, fstab, and session state.

**Read-only** — neither mode takes a lock or writes outside its log tree.

**Unowned state** — `--verify` also reports state the profile does not own: orphaned admin-scope masks, unmanaged `60-ry-*` drop-ins, and any `sdboot-manage.conf.d` drop-in.

## Embedded Values

> [!CAUTION]
> `ry-install.fish` and `ry-verify.fish` carry their shared tunables verbatim and ship at the same version. Clone both repos at the same tag. A version mismatch leaves `ry-verify.fish` checking values `ry-install.fish` no longer deploys.

The expected state: every static and runtime check reads against the keys below. All tunables are `set -g` globals — there is no external config file, and only `EXPECTED_SCALING_DRIVER` is verify-side alone. Every other key is carried verbatim by [ry-install](https://github.com/ryanmusante/ry-install) at the same version; edit both repos in lockstep.

### Bootloader Keys

| Key | Value | Checked as | File |
|---|---|---|---|
| `LOADER_DEFAULT` | `@saved` | `default` | `loader.conf` |
| `LOADER_TIMEOUT` | `0` | `timeout` | `loader.conf` |
| `LOADER_CONSOLE_MODE` | `keep` | `console-mode` | `loader.conf` |
| `LOADER_EDITOR` | `no` | `editor` | `loader.conf` |
| `SDBOOT_DEFAULT_ENTRY` | `manual` | `DEFAULT_ENTRY=` | `sdboot-manage.conf` |
| `SDBOOT_OVERWRITE` | `yes` | `OVERWRITE_EXISTING=` | `sdboot-manage.conf` |
| `SDBOOT_REMOVE_EXISTING` | `yes` | `REMOVE_EXISTING=` | `sdboot-manage.conf` |
| `SDBOOT_REMOVE_OBSOLETE` | `yes` | `REMOVE_OBSOLETE=` | `sdboot-manage.conf` |

### Kernel Parameters

| Token | Effect |
|---|---|
| `amd_pstate=active` | CPPC autonomous mode — the `amd-pstate-epp` scaling driver |
| `btusb.enable_autosuspend=n` | keep the BT controller powered — no reconnect stalls |
| `fsck.mode=auto` | fsck only when the filesystem asks for it |
| `fsck.repair=yes` | auto-repair whatever fsck finds |
| `iommu=pt` | passthrough default domain — low DMA overhead |
| `ipv6.disable=1` | disable the IPv6 stack |
| `mt7925e.disable_aspm=1` | MT7925 endpoint ASPM off — driver-level coredump mitigation |
| `nvme_core.default_ps_max_latency_us=0` | NVMe APST off — no power-state exit latency |
| `pcie_aspm.policy=performance` | bias every PCIe link away from ASPM |
| `processor.max_cstate=1` | cap ACPI C-states at C1 — idle-exit latency floor |
| `quiet` | suppress boot console noise |
| `split_lock_detect=off` | no split-lock throttling penalty in games |
| `usbcore.autosuspend=-1` | USB autosuspend off globally |
| `zswap.enabled=0` | zswap off — zram is the swap path |

### Initramfs

`HOOKS` order is an invariant, enforced when the profile is deployed: `base` first, `fsck` last, no duplicates, and `systemd` before `autodetect`, `keyboard`, and `sd-vconsole`; `autodetect` before `microcode` and `modconf`; `keyboard` before `sd-vconsole`; `modconf` before `kms`; `block` before `filesystems`.

| Key | Value | Checked as |
|---|---|---|
| `MKINITCPIO_MODULES` | `amdgpu` | `MODULES=()` |
| `MKINITCPIO_HOOKS` | `base`, `systemd`, `autodetect`, `microcode`, `modconf`, `kms`, `keyboard`, `sd-vconsole`, `block`, `filesystems`, `fsck` | `HOOKS=()` |
| `MKINITCPIO_COMPRESSION` | `zstd` | `COMPRESSION=` |
| `MKINITCPIO_COMPRESSION_OPTIONS` | `-3` | `COMPRESSION_OPTIONS=()` |

### Service Keys

`DNSOverTLS=` and `DNSSEC=` are left unset by design — the router does DoT upstream and validates DNSSEC. The router serves DoT WAN-side only, so a host `DNSOverTLS=yes` would fail closed.

`NM_WIFI_POWERSAVE` is `2` because the MT7925 handles powersave in software and spikes latency otherwise. `BLACKLIST_AMDXDNA` is `false` because the IOMMU is on; [Tuning Notes](#tuning-notes) has the reverse switch.

| Key | Value | Checked as |
|---|---|---|
| `RESOLVED_MDNS` | `no` | `MulticastDNS=` |
| `RESOLVED_LLMNR` | `no` | `LLMNR=` |
| `NM_DISPATCHER_LOGLEVELMAX` | `notice` | `LogLevelMax=` |
| `COUNTRY` | `US` | `COUNTRY=` |
| `LOGIND_IGNORE_KEYS` | 8 power, suspend, hibernate, and reboot keys | `Handle*Key=ignore` |
| `NM_WIFI_BACKEND` | `wpa_supplicant` | `wifi.backend=` |
| `NM_WIFI_POWERSAVE` | `2` (disabled) | `wifi.powersave=` |
| `NM_LOG_LEVEL` | `WARN` | `level=` |
| `CPUPOWER_GOVERNOR` | `performance` | `GOVERNOR=` |
| `BT_AUTO_ENABLE` | `true` | `AutoEnable=` |
| `BT_FAST_CONNECTABLE` | `true` | `FastConnectable=` |
| `BT_RECONNECT_ATTEMPTS` | `3` | `ReconnectAttempts=` |
| `GPU_DPM_LEVEL` | `high` | udev `ATTR{device/power_dpm_force_performance_level}` |
| `EPP_PREFERENCE` | `performance` | udev `ATTR{cpufreq/energy_performance_preference}` |
| `EXPECTED_SCALING_DRIVER` | `amd-pstate-epp` | nothing — checked at runtime, never written |
| `BLACKLIST_AMDXDNA` | `false` | nothing — `true` emits `blacklist amdxdna` |

### Session Environment

| Variable | Effect |
|---|---|
| `DXVK_LOG_LEVEL=none` | DXVK logging off |
| `GSK_RENDERER=ngl` | GTK4 GL renderer; the Vulkan renderer aborts on gfx1151 |
| `MANGOHUD=1` | HUD on for Vulkan titles |
| `MESA_SHADER_CACHE_MAX_SIZE=16G` | roomy Mesa shader cache |
| `POWERDEVIL_NO_DDCUTIL=1` | PowerDevil DDC/CI off — silences `org_kde_powerdevil` i2c errors |
| `PROTON_FSR4_INDICATOR=1` | on-screen FSR4-active watermark (Proton-CachyOS) |
| `PROTON_LOCAL_SHADER_CACHE=1` | per-prefix shader cache |
| `VKD3D_DEBUG=none` | vkd3d logging off |
| `VKD3D_SHADER_DEBUG=none` | vkd3d shader logging off |
| `WINEDEBUG=-all` | Wine debug channels off |

### Sysctl Overrides

Ships at priority `95`, after the vendor `70-cachyos-settings.conf`.

| Key | Value | Effect |
|---|---|---|
| `kernel.nmi_watchdog` | `0` | NMI watchdog off |
| `net.core.default_qdisc` | `fq` | pairs with BBR |
| `net.ipv4.tcp_congestion_control` | `bbr` | BBR congestion control |
| `net.ipv4.tcp_notsent_lowat` | `16384` | cap unsent buffer at 16 KiB |
| `net.ipv4.tcp_slow_start_after_idle` | `0` | keep the congestion window across idle |
| `vm.compaction_proactiveness` | `0` | proactive compaction off |
| `vm.max_map_count` | `2147483642` | Steam's esync requirement |
| `vm.watermark_boost_factor` | `0` | watermark boosting off |

## Packages

**Expected present** (`PKGS_ADD`, 17) — `nvme-cli`, `cachyos-gaming-meta`, `cachyos-gaming-applications`, `cachyos-benchmarker`, `lib32-mesa`, `mkinitcpio-firmware`, `fd`, `sd`, `dust`, `procs`, `bottom`, `htop`, `lm_sensors`, `rtkit`, `realtime-privileges`, `nftables`, `pacman-contrib`.

**Expected absent** (`PKGS_DEL`, 9) — `plymouth`, `cachyos-plymouth-bootanimation`, `cachyos-plymouth-theme`, `breeze-plymouth`, `plymouth-kcm`, `micro`, `cachyos-micro-settings`, `cachy-update`, `kdeconnect`.

## Units

**Expected masked** (`MASK`, 11) — `ananicy-cpp.service`, `power-profiles-daemon.service`, `NetworkManager-wait-online.service`, `avahi-daemon.service`, `avahi-daemon.socket`, `ufw.service`, `sleep.target`, `suspend.target`, `hibernate.target`, `hybrid-sleep.target`, `suspend-then-hibernate.target`.

**Expected enabled** (`EXPECTED_SERVICES`, 5) — `fstrim.timer`, `NetworkManager.service`, `cpupower.service`, `nftables.service`, `bluetooth.service`.

## Tuning Notes

How the checked values behave at runtime, and the remedy when a check flags one.

### Gaming Stack

- `/dev/ntsync` — reported by `--verify`. Proton reads it directly; `PROTON_NO_NTSYNC=1` opts out at the Proton level.
- `PROTON_FSR4_INDICATOR=1` — Proton-CachyOS watermark confirming FSR4 is active; it sets `FSR_WATERMARK=1` and `FSR_FG_WATERMARK=1` inside the prefix. Proton-EM uses `FSR4_WATERMARK=1` instead.
- `cpu_stats` and `cpu_temp` — shipped commented out; add either on its own line and redeploy with [ry-install](https://github.com/ryanmusante/ry-install). `cpu_custom_temp_sensor` is inert: MangoHud reads `apu_cpu_temp` from `gpu_metrics` first. Zen 5 `cpu_power` is open upstream ([MangoHud #1794](https://github.com/flightlessmango/MangoHud/issues/1794)).

### Kernel Parameter Notes

- `iommu=pt` — IOMMU on for the XDNA NPU, VFIO and SR-IOV; to shed the last DMA-mapping overhead on a box using none of them, add `amd_iommu=off`, set `BLACKLIST_AMDXDNA true`, and redeploy with [ry-install](https://github.com/ryanmusante/ry-install).
- `ipv6.disable=1` — the ruleset carries the ICMPv6 base accept, so the fallback entry still gets working NDP; for dual-stack, drop the token, add any service-specific IPv6 rules, and redeploy with [ry-install](https://github.com/ryanmusante/ry-install).
- `pcie_aspm.policy=performance` — addresses Bluetooth reconnect and NVMe latency; plain `pcie_aspm=off` only inherits the BIOS state.
- `mt7925e.disable_aspm=1` — pairs with `pcie_aspm.policy=performance` at the endpoint driver; coredumps are still reported on the Wi-Fi adapter without it. Drop either token and redeploy with [ry-install](https://github.com/ryanmusante/ry-install) to restore the default.
- `LINUX_FALLBACK_OPTIONS="quiet"` — the fallback entry carries none of the managed kernel parameters, so it boots with the IOMMU on, IPv6 enabled, and firmware-default ASPM. `--verify` skips `*-fallback.conf`.
- `timeout 0` with `default @saved` — with no saved entry (fresh ESP), sd-boot picks by its own sort order and can boot the fallback unseen; hold a key at power-on and select the tuned entry once.

## BIOS

Firmware is not checked — set the flat `SPL = fPPT = sPPT = 85 W` ceiling (stock boosts to 140 W), `STAPM Boost = 0`, and `TjMax = 90 °C` once, under `Advanced → SMU Common Options`; multi-thread gains flatten past ~85 W. Full per-setting walkthrough: [gtr9pro-bios-reference](https://github.com/ryanmusante/gtr9pro-bios-reference).

## Troubleshooting

**Unmanaged 60-ry- drop-in warned** — `pacman -Qo /etc/modprobe.d/*` to confirm ownership, then `sudo rm` the files left by earlier versions.

**Masked unit not in `MASK` reported** — `sudo systemctl unmask <unit>` if an earlier `MASK` masked it; leave distro and hand-made masks alone.

## Contributing

Questions and bug reports: [GitHub issues](https://github.com/ryanmusante/ry-verify/issues). Single-host scope — open an issue before a PR.

## License

MIT — see [LICENSE](LICENSE).
