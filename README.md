# ry-verify

**Version 7.193.0** · [Changelog](CHANGELOG.md)

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

The 17 files are enumerated in [ry-install](https://github.com/ryanmusante/ry-install)'s Managed Files, in deploy order. Each is regenerated in memory and compared byte for byte; system files are checked against `0644` where the filesystem records modes, user files against `0600`.

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

The expected state: every check reads against the keys `ry-verify.fish` embeds. Value tables, package and unit sets, and tuning rationale live in [ry-install](https://github.com/ryanmusante/ry-install) at the same tag; the two keys below are verify-side alone. Edit both repos in lockstep.

### Verify-only Keys

| Key | Value | Checked as |
|---|---|---|
| `EXPECTED_SCALING_DRIVER` | `amd-pstate-epp` | nothing — checked at runtime, never written |
| `EXPECTED_VULKAN_PKGS` | `vulkan-radeon`, `lib32-vulkan-radeon` | presence, with `PKGS_ADD` (Static: packages) |

## BIOS

Firmware is not checked — the assumed ceiling and the per-setting walkthrough live in [ry-install](https://github.com/ryanmusante/ry-install)'s BIOS section.

## Troubleshooting

**Unmanaged 60-ry- drop-in warned** — `pacman -Qo /etc/modprobe.d/*` to confirm ownership, then `sudo rm` the files left by earlier versions.

**Masked unit not in `MASK` reported** — `sudo systemctl unmask <unit>` if an earlier `MASK` masked it; leave distro and hand-made masks alone.

## Contributing

Questions and bug reports: [GitHub issues](https://github.com/ryanmusante/ry-verify/issues). Single-host scope — open an issue before a PR.

## License

MIT — see [LICENSE](LICENSE).
