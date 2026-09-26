# ry-verify

**Version 7.217.0** · [Changelog](CHANGELOG.md)

Standalone audit of the GTR9 Pro CachyOS profile that [ry-install](https://github.com/ryanmusante/ry-install) deploys. `ry-verify.fish` regenerates all 17 [Managed Files](#managed-files) in memory, compares the installed bytes, then reads live kernel-cmdline, module, sysctl, unit, fstab, and session state — `--verify` reports every check, `--report` adds an HTML report of the run, `--check` probes silently for drift.

## Quick Start

> [!WARNING]
> Run as your normal user — never run the script itself with `sudo`. Meet [Requirements](#requirements) first. Verify after a reboot.

```fish
git clone https://github.com/ryanmusante/ry-verify.git
cd ry-verify
chmod +x ry-verify.fish
sudo -v
./ry-verify.fish
```

A run closes with `VERIFICATION SUMMARY` and a `Results:` line counting `OK`, `WARN`, `FAIL`, and `GEN_FAIL`; `--report` then prints `[INFO] Report: <path>` — see [Exit Codes](#exit-codes).

## Requirements

| Requirement | Detail |
|---|---|
| OS | CachyOS (Arch-based), systemd-boot with BLS entries |
| Shell | fish 3.6 or newer |
| Hardware | CPU matching `Ryzen AI Max` — bypass via [Environment Overrides](#environment-overrides) |
| BIOS | flat 85 W ceiling, `TjMax = 90 °C` — see [BIOS](#bios) |
| Privileges | normal user with sudo rights; `sudo -v` cached before the run |
| Tools | GNU coreutils, `findmnt`, `awk`, `grep`, `find`, `pacman`, `systemctl` |

## Usage

The bare invocation equals `--verify`; `--report` runs `--verify` and writes the [Report](#report); `--check` is the silent idempotency probe (the three are mutually exclusive). `--install-file` belongs to [ry-install](https://github.com/ryanmusante/ry-install) and is an unknown option here, exit `2`. Positional arguments exit `2`. `--help` (`-h`) and `--version` (`-v`) are the only stdout output — every result goes to stderr.

Each run writes one JSONL log (`0600`) to `~/ry-install/logs/YYYY-MM-DD/MODE-YYYYMMDD-HHMMSS±ZZZZ-PID.jsonl`, where `MODE` is `verify`, `check`, or `report`; `--report` adds `report-YYYYMMDD-HHMMSS±ZZZZ-PID.html` (`0600`) beside its log, same stem. A fresh install's `./ry-verify.fish --check` reports drift until reboot.

## Exit Codes

| Code | Meaning |
|---|---|
| `0` | OK — success, `WARN`-only runs, and a clean `--check` |
| `1` | a `--verify` or `--report` mismatch, or a report that could not be written |
| `2` | bad arguments, root misuse |
| `3` | missing dependency, uncached sudo, gate mismatch; `--check` stays silent |
| `10` | drift — `--check` found drift from the managed baseline |

## Environment Overrides

Skipping the hardware check is the risky override — a wrong-CPU run compares against an incorrect kernel cmdline and initramfs `MODULES`.

| Variable | Effect |
|---|---|
| `RY_INSTALL_SKIP_HARDWARE_CHECK=1` | bypass the `EXPECTED_CPU_MATCH` hard-fail |
| `NO_COLOR` | disable colored output when set to a non-empty value ([no-color.org](https://no-color.org)) |

## Managed Files

The 17 files are enumerated in [ry-install](https://github.com/ryanmusante/ry-install)'s Managed Files, in deploy order. System files are checked against `0644` where the filesystem records modes, user files against `0600`.

## Checks

`--verify` runs the static groups first, then the runtime groups; `--check` runs the silent subset and reports drift only.

| Group | Scope |
|---|---|
| Static: boot | `loader.conf`, `sdboot-manage.conf`, `sdboot-manage.conf.d` drop-ins that outrank it, `/etc/kernel/cmdline` (`KERNEL_PARAMS`, `root=UUID`, `rw`), `mkinitcpio.conf`, `$BOOT` entries |
| Static: system | resolved, logind, NetworkManager dispatcher logging, NetworkManager, `iw-regdomain`, bluetooth, `cpupower-service.conf`, sysctl drop-in, udev, modprobe plus the unmanaged `60-ry-*` sweep, nftables |
| Static: user | `environment.d` (`ENV_VARS`), MangoHud |
| Static: packages | `PKGS_ADD` and `EXPECTED_VULKAN_PKGS` present, `PKGS_DEL` absent, `pacman.conf` `IgnorePkg` and `ParallelDownloads` |
| Static: services | `MASK` unit state, plus masked units the profile no longer declares |
| Static: syntax | live `mkinitcpio.conf` `HOOKS` presence — ordering is not re-checked here |
| Static: checksum | installed bytes compared with generator output, a symlinked destination rejected rather than followed, root-UUID fallback compare, `.ry.bak` copies in `~/ry-install/backups/` non-empty |
| Runtime: kernel | live `/proc/cmdline`, kernel parser rejections, GPU DPM level, CPU governor, EPP, `EXPECTED_SCALING_DRIVER` and boost, module parameters, NVMe I/O scheduler, blacklists |
| Runtime: services | `conf.d`-implied and `EXPECTED_SERVICES` units, `MASK` units inactive, user-scope units, Wi-Fi and NM backend |
| Runtime: environment | session `ENV_VARS`, live sysctl via `/proc/sys`, fstab ext4 entries, live ext4 mount options, `/dev/ntsync`, wireless regulatory domain |
| Runtime: session | NetworkManager system-connections perms, installed file modes, parent directories of managed files |

## Report

`--report` runs every `--verify` check, then renders the run into one self-contained HTML file beside its JSONL log — inline styles and SVG charts, no scripts, no network. Open it in any browser; print it to PDF for a portable copy. Sections run from most to least urgent:

| Section | Content |
|---|---|
| Verdict | `PASS`, `PASS-WITH-WARNINGS`, `FAIL`, or `PREFLIGHT`, a counts ring, the result lines, and run metadata |
| Action items | every `FAIL`, then every `WARN`, with its group and any `INFO` line logged directly after it |
| System | host, firmware, OS, kernel, CPU scaling state and per-CPU clocks, GPU IDs and clocks, VRAM, GTT, RAM, swap, and storage meters, displays, key package versions |
| Profile changes | the 17 [Managed Files](#managed-files) regenerated and compared byte for byte, `KERNEL_PARAMS` deployed and live, `SYSCTL_VALUES`, `ENV_VARS`, packages, units, embedded keys, and a coverage chart |
| Verification ledger | a per-group chart, then every logged row, groups with a `FAIL` first, then groups with a `WARN` |
| Appendix | every structured log event, each generated file body with its SHA-256, and the method |

System facts are read without sudo; installed system files are read with `sudo -n` for the byte compare. A report that cannot be written prints `[ERR] Report not written`, logs `REPORT_WRITE_FAIL`, and turns an otherwise clean exit into `1`.

Profile-change states are graded as the ledger grades the same finding: `match`, `active`, `present`, `enabled`, `masked`, and `removed` pass; `not installed`, `not set`, `knob absent`, `unreadable`, `still installed`, `no user bus`, `no root UUID`, and a unit running but not enabled warn; everything else fails — a managed file that differs or cannot be read, a deployed kernel parameter not yet live, a masked unit still active. The unit table grades the unit file; whether an enabled unit is running is the ledger's `Runtime: services` group. The coverage chart counts passing rows only.

## Safety and Reliability

**Read-only** — no mode takes a lock or writes outside its log tree; the report is written there too.

**Unowned state** — `--verify` also reports state the profile does not own: orphaned admin-scope masks, unmanaged `60-ry-*` drop-ins, and any `sdboot-manage.conf.d` drop-in.

## Embedded Values

> [!CAUTION]
> `ry-install.fish` and `ry-verify.fish` carry their shared tunables verbatim and ship at the same version. Clone both repos at the same version. A version mismatch leaves `ry-verify.fish` checking values `ry-install.fish` no longer deploys.

Value tables, package and unit sets, and tuning rationale live in [ry-install](https://github.com/ryanmusante/ry-install); the two keys below are verify-side alone. Edit both repos in lockstep.

### Verify-only Keys

| Key | Value | Checked as |
|---|---|---|
| `EXPECTED_SCALING_DRIVER` | `amd-pstate-epp` | live scaling driver (Runtime: kernel) |
| `EXPECTED_VULKAN_PKGS` | `vulkan-radeon`, `lib32-vulkan-radeon` | presence, with `PKGS_ADD` (Static: packages) |

## BIOS

Firmware is not checked — the assumed ceiling and the per-setting walkthrough live in [ry-install](https://github.com/ryanmusante/ry-install)'s BIOS section.

## Troubleshooting

**Unmanaged 60-ry- drop-in warned** — `pacman -Qo /etc/modprobe.d/*` to confirm ownership, then `sudo rm` the files left by earlier versions.

**Masked unit not in `MASK` reported** — `sudo systemctl unmask <unit>` if an earlier `MASK` masked it; leave distro and hand-made masks alone.

**Report not written** — the HTML is rendered into a temporary file in the log directory and renamed into place, so `~/ry-install/logs/YYYY-MM-DD/` must accept a new file: check free space and the directory mode (`0700`). The JSONL log records `REPORT_WRITE_FAIL` with the reason; the checks themselves are unaffected.

## Contributing

Questions and bug reports: [GitHub issues](https://github.com/ryanmusante/ry-verify/issues). Single-host scope — open an issue before a PR.

## License

MIT — see [LICENSE](LICENSE).
