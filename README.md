# ry-verify

**Version 7.241.0** · [Changelog](CHANGELOG.md)

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

Each section, static then runtime, ends with `VERIFICATION SUMMARY` and a `Results:` line counting its own `OK`, `WARN`, `FAIL`, and `GEN_FAIL`; the run closes with a `Combined (static + runtime):` line totalling both, or `Combined (startup + static + runtime):` when startup warnings count too; `--report` then prints `[INFO] Report: <path>` — see [Exit Codes](#exit-codes).

## Requirements

| Requirement | Detail |
|---|---|
| OS | CachyOS (Arch-based), systemd-boot with BLS entries |
| Shell | fish 3.6 or newer |
| Hardware | CPU matching `Ryzen AI Max` — on any other CPU `--verify` and `--report` warn and continue; `--check` exits `3` unless overridden via [Environment Overrides](#environment-overrides) |
| BIOS | flat 85 W ceiling, `TjMax = 90 °C` — see [BIOS](#bios) |
| Privileges | normal user with sudo rights; `sudo -v` cached before the run — when it is not, `--verify` and `--report` prompt through `sudo -v` if stdin and stderr are both a TTY, and exit `3` otherwise or if the prompt fails; `--check` never prompts and exits `3` |
| Tools | GNU coreutils, `findmnt`, `awk`, `grep`, `find`, `pacman`, `systemctl` |

## Usage

The bare invocation equals `--verify`; `--report` runs `--verify` and writes the [Report](#report); `--check` is the silent idempotency probe (the three are mutually exclusive). `--install-file` belongs to [ry-install](https://github.com/ryanmusante/ry-install) and is an unknown option here, exit `2`. Positional arguments exit `2`. `--help` (`-h`) and `--version` (`-v`) are the only stdout output — every result goes to stderr.

Each run writes one JSONL log (`0600`) to `~/ry-install/logs/YYYY-MM-DD/MODE-YYYYMMDD-HHMMSS±ZZZZ-PID.jsonl`, where `MODE` is `verify`, `check`, or `report`; `--report` adds `report-YYYYMMDD-HHMMSS±ZZZZ-PID.html` (`0600`) beside its log, same stem. A fresh install's `./ry-verify.fish --check` reports drift until reboot.

## Exit Codes

| Code | Meaning |
|---|---|
| `0` | OK — success, `WARN`-only runs, and a clean `--check` |
| `1` | a `--verify` or `--report` mismatch, a report that could not be written, or `-h`/`-v` output that could not be written (stdout closed or full) |
| `2` | bad arguments, root misuse — except a valid `--check` run as root, which exits `3` silently |
| `3` | missing dependency; uncached sudo that no TTY prompt resolved; gate mismatch; under `--check`, a CPU not matching `EXPECTED_CPU_MATCH` unless [overridden](#environment-overrides); `--check` stays silent |
| `10` | drift — `--check` found drift from the managed baseline |

## Environment Overrides

Skipping the hardware check is the risky override — a wrong-CPU run compares against an incorrect kernel cmdline and initramfs `MODULES`.

- `RY_INSTALL_SKIP_HARDWARE_CHECK=1` — let `--check` probe a CPU that does not match `EXPECTED_CPU_MATCH`, or whose model is unreadable, instead of exiting `3`
- `NO_COLOR` — disable colored output when set to a non-empty value ([no-color.org](https://no-color.org))

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
| Static: checksum | installed bytes compared with generator output, a symlinked destination rejected rather than followed, root-UUID fallback compare, `.ry.bak` copies in `~/ry-install/backups/` non-empty, stray files beside managed files |
| Runtime: kernel | live `/proc/cmdline`, kernel parser rejections, GPU DPM level, CPU governor, EPP, `EXPECTED_SCALING_DRIVER` and boost, module parameters, NVMe I/O scheduler, blacklists |
| Runtime: services | `conf.d`-implied and `EXPECTED_SERVICES` units, live nftables input policy drop and IPv4 ping accept, `MASK` units inactive, user-scope units, firewall posture, Wi-Fi, NM backend, unmanaged Wi-Fi P2P device |
| Runtime: environment | session `ENV_VARS`, live sysctl via `/proc/sys`, fstab ext4 entries, live ext4 mount options, `/dev/ntsync`, wireless regulatory domain |
| Runtime: session | NetworkManager system-connections perms, installed file modes, parent directories of managed files (a symlinked directory is checked at its target) |

## Report

`--report` runs every `--verify` check, then renders the run into one self-contained HTML file beside its JSONL log — inline styles and SVG charts, no scripts, no network. Sections run from most to least urgent:

| Section | Content |
|---|---|
| Verdict | `PASS`, `PASS-WITH-WARNINGS`, `FAIL`, or `PREFLIGHT`, a counts ring, the result lines, and run metadata |
| Action items | every `FAIL`, then every `WARN`, with its group and any `INFO` line logged directly after it |
| System | host, firmware, OS, kernel, CPU scaling state and per-CPU clocks, GPU IDs and clocks, VRAM, GTT, RAM, swap, and storage meters, displays, key package versions |
| Profile changes | the 17 [Managed Files](#managed-files) regenerated and compared byte for byte, `KERNEL_PARAMS` deployed and live, `SYSCTL_VALUES`, `ENV_VARS`, packages, units, embedded keys, and a coverage chart |
| Verification ledger | a per-group chart, then every logged row, groups with a `FAIL` first, then groups with a `WARN` |
| Appendix | every structured log event, each generated file body with its SHA-256, and the method |

A report that cannot be written prints `[ERR] Report not written`, logs `REPORT_WRITE_FAIL`, and turns an otherwise clean exit into `1`.

Profile-change states are graded as the ledger grades the same finding: `match`, `active`, `present`, `enabled`, `masked`, and `removed` pass; `not set`, `knob absent`, `unreadable`, `no sudo`, `still installed`, `no user bus`, `no root UUID`, a unit to enable that is `not installed`, and a unit running but not enabled warn; a unit to mask that is `not installed` is neutral; everything else fails — a managed file that differs or cannot be read, a deployed kernel parameter not yet live, a masked unit still active. With no user bus, the ledger warns once for each check it skips — session `ENV_VARS` and user units — and the report marks every `ENV_VARS` row. The unit table grades the unit file; whether an enabled unit is running is the ledger's `Runtime: services` group. The coverage chart counts passing rows out of graded rows; a neutral row counts toward neither.

## Safety and Reliability

**Read-only** — no mode takes a lock or writes outside its log tree, `~/ry-install/logs/`, kept `0700`; nothing lands in `/tmp`. A missing `~/ry-install` is created `0700` to hold it; an existing one is never re-moded — a group- or world-writable one stops the run, exit `3`.

**Unowned state** — `--verify` also reports state the profile does not own: orphaned admin-scope masks, unmanaged `60-ry-*` drop-ins, any `sdboot-manage.conf.d` drop-in, and stray files beside managed files.

## Embedded Values

> [!CAUTION]
> `ry-install.fish` and `ry-verify.fish` carry their shared tunables verbatim and ship in lockstep; clone both repos at the same version. A version mismatch leaves `ry-verify.fish` checking values `ry-install.fish` no longer deploys.

The length of each array among the shared tunables is a drift tripwire in `_ir_validate_counts` of both scripts — `KERNEL_PARAMS:15` among them; the verify-only `EXPECTED_VULKAN_PKGS` below is pinned, as `EXPECTED_VULKAN_PKGS:2`, in `ry-verify.fish` alone, and the accepted-value sets `_RY_DPM_LEVELS` and `_RY_EPP_LEVELS` carry no pin. Adding or dropping an element, such as a kernel token per [ry-install](https://github.com/ryanmusante/ry-install)'s Kernel Parameter Notes, also means updating that `NAME:<n>` in each script that pins it; otherwise that script refuses to run with `NAME count drift` and exits `3`.

Value tables, package and unit sets, and tuning rationale live in [ry-install](https://github.com/ryanmusante/ry-install); the two keys below exist only on the verify side.

### Verify-only Keys

- `EXPECTED_SCALING_DRIVER` = `amd-pstate-epp` — live scaling driver (Runtime: kernel)
- `EXPECTED_VULKAN_PKGS` = `vulkan-radeon`, `lib32-vulkan-radeon` — presence, with `PKGS_ADD` (Static: packages)

## BIOS

Firmware is not checked — the assumed ceiling and the per-setting walkthrough live in [ry-install](https://github.com/ryanmusante/ry-install)'s BIOS section.

## Troubleshooting

**Unmanaged 60-ry- drop-in warned** — `pacman -Qo /etc/modprobe.d/*` to confirm ownership, then `sudo rm` the files left by earlier versions.

**Masked unit not in `MASK` reported** — `sudo systemctl unmask <unit>` if an earlier `MASK` masked it; leave distro and hand-made masks alone.

**Stray file reported** — neither the profile nor a package owns it (package files print as `Package file:`); merge or delete a `.pacnew` sibling, and remove anything else once nothing uses it.

**Report not written** — `~/ry-install/logs/YYYY-MM-DD/` must accept a new file: check free space and the directory mode (`0700`). The JSONL log records `REPORT_WRITE_FAIL` with the reason.

## Contributing

Questions and bug reports: [GitHub issues](https://github.com/ryanmusante/ry-verify/issues). Single-host scope — open an issue before a PR.

## License

MIT — see [LICENSE](LICENSE).
