Changes for ry-verify
=====================

Newest first. Versioning is MAJOR.MINOR.PATCH.

7.261.0
-------

  - readme: the Results line counts WARN, FAIL and GEN_FAIL only when present
  - changelog: fold 7.258.0 into 7.246.0 - 7.258.0; merge related bullets


7.260.0
-------

  - guard: fish_read_limit is pinned to 1 GiB, the fish 4 default; 100 MiB,
    the fish 3 default, cut fish 4's limit tenfold
  - check: an empty /proc/cmdline that reads cleanly is drift in every token
    (exit 10), as --verify FAILs them, not a probe failure (exit 3)
  - verify: an ESP not mounted at /boot, an XBOOTLDR $BOOT or a non-vfat
    /boot fallback FAILs the loader.conf section, as ry-install refuses them
  - verify: a linux boot entry with no initrd line FAILs, as ry-install's
    boot sanity now does: its kernel would boot without an initramfs
  - verify: a boot entry path holding .. or a symlink out of $BOOT FAILs, as
    ry-install's boot sanity does, not a file outside $BOOT read as present
  - verify: an ESP holding only fallback entries FAILs: no entry carries
    KERNEL_PARAMS for the next boot
  - verify: boot entries are read as systemd-boot reads them: the last linux
    line counts, and auto-* entries are skipped
  - verify: /boot paths are probed only as root (file presence, per-path
    fstype), as a 0700 ESP requires
  - verify: a managed file, loader entry or entries listing that fails to
    read with sudo valid FAILs, not a WARN blaming a possible lapse
  - verify: an unlistable drop-in, preset, modprobe.d or NetworkManager
    connections directory names a sudo lapse or a read error
  - verify: /etc/kernel/cmdline reads empty or unreadable (with cat's rc),
    not one message for both
  - verify: with the root UUID unresolved, a missing or unreadable
    /etc/kernel/cmdline FAILs, ledger and report, not as a skipped checksum
  - verify: the mkinitcpio.conf amdgpu row follows MKINITCPIO_MODULES; a
    tunable that drops amdgpu no longer FAILs a matching file
  - verify: mkinitcpio.conf reads tell a sudo lapse (one WARN) and a failed
    read (one FAIL) from a missing assignment
  - verify: an empty mkinitcpio.conf HOOKS array FAILs under syntax
    validation
  - verify: a managed file reduced to its [section] header FAILs each key as
    missing, not as a file with no non-comment lines
  - verify: a user or group name holding a space no longer FAILs the file
    owner check, nor a user name holding one the parent-directory check
  - verify: a parent-directory owner FAIL quotes the owner in its chown
    hint, so a name holding a space pastes as one argument
  - verify: split_lock_detect=, read raw at boot, is INFO when an older
    kernel lists it as unknown; other complaints are still counted
  - verify: amd_pstate status is checked against the KERNEL_PARAMS
    amd_pstate= value, and skipped when the token is absent
  - verify: a kernel token already REJECTED or read raw gets no second WARN
    as a quoted-value match
  - report: with no sudo binary, system-file rows read no sudo, and the
    report never tries to run sudo
  - report: the kernel-parameter table shows a dash for an unreadable
    kernel cmdline (Deployed) or /proc/cmdline (Live)
  - report: a disabled nftables.service whose ruleset cannot be probed reads
    no sudo (WARN), as the ledger grades it
  - logging: an absent managed resolved drop-in logs RESOLVED_RUNTIME_SKIP
  - logging: a raw split_lock_detect= read logs KPARAM_RAW_READ; an ESP
    layout FAIL logs VERIFY_ESP_LAYOUT_FAIL
  - logging: with the root UUID unresolved, a failed read logs
    VERIFY_STATIC_READ_FAIL and a lapsed one VERIFY_STATIC_SUDO_LAPSE
  - logging: a log dir moded to 0700 logs STATE_DIR_PERM_FIX, as ry-install
    does, not LOG_DIR_PERM_FIX
  - consistency: the GKeyFile rows read parsable, as the other parse
    messages do


7.246.0 - 7.258.0
-----------------

  - env: 7.248.0 expect 0x3434/0x0b00 (Keychron Q0 HE) in
    SDL_GAMECONTROLLER_IGNORE_DEVICES
  - configuration: 7.248.0 expect the Xbox Wireless Controller libinput ignore
  - configuration: 7.251.0 the 60-ry-modules.conf header no longer claims
    module options, matching ry-install
  - configuration: 7.257.0 with an hfp_*/hsp_* role in WP_BLUEZ_ROLES, the
    expected drop-in comment no longer says there is none, matching ry-install
  - guard: 7.250.0 internal state inherited from the environment or
    config.fish is erased at startup
  - guard: 7.250.0 a fish universal named like internal state (the error names
    its set -eU) or regex-easyesc turned off exits 3; --check stays silent
  - guard: 7.250.0 an early exit or a signal before the JSONL header removes
    only the empty log and ~/ry-install dirs this run created
  - guard: 7.250.0 an early exit no longer runs a user's fish function named
    _do_cleanup; 7.251.0 nor one named _dc_settle or _write_footer
  - guard: 7.250.0 a config.fish job-control or IFS setting no longer steers a
    run; 7.258.0 fish_read_limit is pinned to 100 MiB
  - guard: 7.250.0 a fish universal named like the tmpfile list or the
    boot-path, kernel-config or package cache is never read
  - guard: 7.250.0 a fish universal named _early_ch is no longer erased
  - guard: 7.251.0 an invalid root UUID never lets a fish universal of that
    name steer the cmdline checks
  - signals: 7.250.0 a second signal while a startup signal is handled no
    longer exits 0 or 1, skipping the dir cleanup
  - signals: 7.250.0 a signal in an early exit's final step, 7.251.0 or before
    the main handler takes over, keeps its exit code; teardown keeps 128+N
  - signals: 7.258.0 a signal while a section's counts are carried or folded
    no longer counts those rows twice in the JSONL footer
  - cli: 7.252.0 an unexpected positional argument prints escaped, an empty
    one as ''; 7.254.0 a root run escapes each leftover argument whole
  - cli: 7.254.0 argument errors print control characters escaped or as ?, C1
    and raw bytes included
  - cli: 7.254.0 -h or -v ending in a newline is an unknown option (exit 2),
    not help
  - preflight: 7.247.0 MKINITCPIO_MODULES and MKINITCPIO_HOOKS tokens are
    charset-checked
  - preflight: 7.252.0 a HOME that normalizes to empty or a non-directory
    names the value set in its exit-3 error
  - preflight: 7.252.0 the CPUPOWER_GOVERNOR refusal names the ry-install
    check whose domain it enforces
  - preflight: 7.254.0 an embedded value ending in a newline (governor,
    country, count, kernel or HOOKS token) is refused
  - preflight: 7.254.0 a relative HOME, 7.255.0 or one with a control
    character, takes the passwd entry; such a passwd entry exits 3
  - preflight: 7.254.0 the root UUID takes findmnt's first match, so a mount
    stacked on / no longer leaves it unresolved
  - preflight: 7.255.0 sudo uncached with no TTY prints one ERR naming the TTY
  - preflight: 7.255.0 the --check hardware override hint names the script as
    invoked, not ./ry-verify.fish
  - preflight: 7.256.0 WP_BLUEZ_ROLES entries must be PipeWire role names;
    WP_BLUEZ_ROLES:4 is a drift tripwire
  - preflight: 7.257.1 an EXPECTED_VULKAN_PKGS member in PKGS_DEL or starting
    with a dash refuses to run, as ry-install now installs the list
  - preflight: 7.258.0 true(1) and test(1), which every sudo -n probe runs,
    are checked at startup (exit 3; --check stays silent)
  - check: 7.247.0 a masked MASK unit still running is drift, 7.255.0 as is
    one activating, reloading or refreshing; an expected unit reloading is not
  - check: 7.250.0 CHECK_UNIT_DRIFT for an inactive or not-enabled unit names
    the expected state; 7.251.0 a not-found one load=not-found expected=loaded
  - check: 7.250.0 a managed symlink behind a directory this user cannot
    traverse no longer records its link mode as drift
  - check: 7.251.0 nftables.service is probed for a live ruleset even when a
    RemainAfterExit drop-in keeps the unit active
  - check: 7.252.0 a disabled systemd-resolved, which ry-install never
    enables, logs CONFD_UNIT_NOT_ENABLED, not drift no re-run clears
  - check: 7.252.0 with no sudo binary, an unlistable modprobe.d no longer
    writes to stderr
  - check: 7.255.0 a unit no re-run installs reads as not installed, as the
    ledger WARNs it, not drift; a system file not owned by root:root is drift
  - check: 7.257.0 CHECK_MODE_DRIFT names the mode as 0666, not 666, like
    expected=
  - check: 7.258.0 a sudo lapse at a managed file's mode or owner probe exits
    3, no verdict, rather than reading as no drift
  - check: 7.258.0 a content difference only in bytes after a NUL is logged
    reason=size, not reason=content
  - check: 7.258.0 a MASK unit whose file is gone but which still runs is
    drift, as ry-install now masks and stops it
  - verify: 7.247.0 a running MASK unit that is not masked is named as such;
    the nftables policy-drop FAIL names the restart
  - verify: 7.247.0 efi and chainload loader entries are skipped; a stale
    60-ry-* drop-in under a symlinked modprobe.d is found
  - verify: 7.247.0 a system file behind a directory this user cannot traverse
    is checked with sudo; a dotfile read failure says to check owner and mode
  - verify: 7.247.0 a missing realpath no longer prints fish errors in the
    stray sweep; the MASK state check is one pass
  - verify: 7.249.0 CPU scaling driver, governor and EPP get one row each
    across all policies, value shown; 7.250.0 a failing one names the expected
  - verify: 7.250.0 the systemd-resolved check runs behind a directory this
    user cannot traverse and warns when a lapsed sudo hides its drop-in
  - verify: 7.250.0 a symlinked mkinitcpio.conf.d no longer warns about
    drop-ins mkinitcpio never reads
  - verify: 7.250.0 a mkinitcpio preset _config is reported: a WARN when it
    names another file, an INFO for mkinitcpio.conf (conf.d skipped)
  - verify: 7.250.0 a dangling link at a managed file's .ry.bak or .ry.orig
    name is listed as a stray, 7.258.0 as is one at /etc/fstab's
  - verify: 7.250.0 a directory at a .ry.bak name in ~/ry-install/backups
    WARNs, 7.251.0 only for the boot files and fstab ry-install backs up
  - verify: 7.250.0 a stale 60-ry-* drop-in behind a 0700 /etc/modprobe.d is
    reported, and the stray sweep skips only reported ones
  - verify: 7.250.0 NetworkManager keyfile modes are checked behind a 0700
    /etc/NetworkManager and without a .nmconnection suffix
  - verify: 7.250.0 a HOOKS build script behind a 0700 /etc/initcpio/install
    no longer reads as missing; 7.251.0 sudo probes only unseen ones
  - verify: 7.250.0 a MISSING row with a label names the expected line
  - verify: 7.251.0 a 0700 mkinitcpio.conf.d or sdboot-manage.conf.d is listed
    as root, not skipped with a WARN
  - verify: 7.251.0 the preset scan skips an empty _config or trailing
    comment, reads names with digits; its INFO no longer contradicts the WARN
  - verify: 7.251.0 a lapsed sudo cache behind a 0700 /etc/NetworkManager
    WARNs, not an INFO that the connections directory is missing
  - verify: 7.252.0 presets in a 0700 /etc/mkinitcpio.d or 0600 files are read
    as root; a custom config several presets name is listed once
  - verify: 7.252.0 an empty .ry.bak reports only for the boot files and fstab
    ry-install backs up
  - verify: 7.252.0 a drop-in directory it cannot list names the cause
  - verify: 7.252.0 a linux boot entry with no options line FAILs as missing
    every KERNEL_PARAMS token, not a WARN
  - verify: 7.254.0 presets behind an x-only /etc/mkinitcpio.d are read as
    root too, and hidden presets are skipped as mkinitcpio skips them
  - verify: 7.254.0 HOOKS, MODULES, COMPRESSION and its options match as whole
    tokens (systemd-tool is not systemd, zstdmt not zstd)
  - verify: 7.254.0 the hook existence check strips quotes and reads a HOOKS
    line without parentheses
  - verify: 7.254.0 a missing BINARIES= or FILES= line FAILs naming the line,
    not an empty found value
  - verify: 7.254.0 a whole-line check FAILs a trailing # comment where the
    parser keeps it in the value (systemd, sysctl, NM, BlueZ, environment.d)
  - verify: 7.254.0 an initramfs array still open at EOF reads as empty, not
    as an earlier assignment
  - verify: 7.254.0 an unlistable /etc/modprobe.d WARNs that the 60-ry-* sweep
    was skipped instead of reporting none
  - verify: 7.254.0 a GPU DPM mismatch counts one FAIL per card, its dmesg
    hint no longer a WARN; an unreadable level WARNs instead of FAILing empty
  - verify: 7.254.0 bool module parameters accept every kstrtobool spelling,
    such as on/off and true/false
  - verify: 7.254.0 - 7.255.0 LINUX_OPTIONS reads as bash reads it: indented
    or single-quoted counts, and a repeated line WARNs and uses the last
  - verify: 7.254.0 - 7.255.0 an unreadable cpufreq attribute or amdgpu
    parameter WARNs, not a FAIL with an empty value
  - verify: 7.254.0 - 7.255.0 a masked unit activating, reloading or
    refreshing FAILs as active; 7.255.0 a running one FAILs once, at runtime
  - verify: 7.254.0 - 7.255.0 fstab mount points decode and resolve as mount
    does: \040, extra slashes, backslashes, symlinks and .. hide no mount
  - verify: 7.255.0 a udev rule with a trailing # comment reads as dropped;
    7.258.0 \ lines are joined and an unparsable rule is dropped, as udev does
  - verify: 7.255.0 key=value rows read keys as their parser does: last
    assignment, blanks at =, section, continued lines, quotes, sysctl forms
  - verify: 7.255.0 MangoHud lines end at the first #, as MangoHud reads them
  - verify: 7.255.0 a HOOKS line's trailing backslash is no hook and HOOKS+=()
    or MODULES+=() appends; 7.257.0 so does a multi-line KEY+=( ... )
  - verify: 7.255.0 the live fstab check reads a stacked target's first mount,
    is not OK with no managed option to compare, and names a sudo lapse
  - verify: 7.255.0 loader entries list as systemd-boot lists them (hidden
    skipped, .conf any case); a $BOOT of only efi or chainload entries FAILs
  - verify: 7.255.0 an unresolvable $BOOT counts once, as the boot entries
    WARN, not also as an ERR
  - verify: 7.255.0 IgnorePkg and ParallelDownloads come from pacman-conf
    (Include= counts, 1 reads set); 7.258.0 a set IgnorePkg reads INFO
  - verify: 7.255.0 a MASK unit removed while running FAILs as running, not
    INFO not installed
  - verify: 7.255.0 an expected unit reloading or refreshing counts as active
  - verify: 7.255.0 the kernel parameter scan reads dmesg when the journal
    lacks the boot lines, and WARNs instead of OK when neither holds them
  - verify: 7.255.0 a failed iw reg get reads as unqueryable, not a wrong
    country with a remedy that fails the same way
  - verify: 7.255.0 with NetworkManager down, the Wi-Fi rows say nmcli failed
    instead of reporting the P2P device unlisted
  - verify: 7.255.0 an unreadable /proc/cmdline WARNs once instead of FAILing
    every kernel token
  - verify: 7.255.0 the root filesystem row takes findmnt's first entry, so a
    mount stacked on / no longer repeats the type
  - verify: 7.255.0 stray, drop-in, preset and stale 60-ry-* names list
    NUL-safe and print control characters as ?, as symlinked-dir targets do
  - verify: 7.255.0 a HOME under /etc or /boot no longer reads the user files
    as system files (sudo, stray scan, report scope)
  - verify: 7.255.0 the NetworkManager connection count skips the files
    keyfile ignores (hidden, ~ backups, temp, .pem, .der)
  - verify: 7.256.0 check the WirePlumber bluez5.roles drop-in; WARN on other
    config that sets bluez5.roles or overrides monitor.bluez.properties
  - verify: 7.256.0 WARN when a connected Bluetooth card offers an HFP/HSP
    profile or its profile list is unreadable (pactl); no user bus WARNs
  - verify: 7.257.0 root=UUID= in /etc/kernel/cmdline must be a whole token;
    an empty, readable file FAILs even after a sudo lapse
  - verify: 7.257.0 a bracketed line in a sectionless file no longer hides a
    later override of a managed key (sysctl, env, loader, sdboot, MangoHud)
  - verify: 7.257.0 a sudo lapse at the ParallelDownloads read is named as
    such (PACMAN_CONF_SUDO_LAPSE), not as a pacman-conf failure
  - verify: 7.257.0 a failed fstab scan (ext4 or malformed rows) WARNs in both
    fstab checks rather than reading as nothing found
  - verify: 7.257.0 an hfp_*/hsp_* role in WP_BLUEZ_ROLES skips the
    headset-profile check; no OK or drop-in row then misstates bluez5.roles
  - verify: 7.257.0 a sudo lapse no longer adds a "No installed files" or "No
    parent directories" WARN; a blank ufw state reads unknown
  - verify: 7.257.0 each linux entry's kernel and initrd must exist non-empty
    in $BOOT, a FAIL naming the restore command; 7.258.0 a directory FAILs
  - verify: 7.257.0 a sudo lapse at the HOOKS check WARNs per hook instead of
    FAILing it NOT FOUND
  - verify: 7.257.0 a session variable set to an empty value FAILs as
    differing (in the report too), not a NOT SET WARN advising a re-login
  - verify: 7.258.0 /etc/iw-regdomain is read as iw-set-regdomain reads it:
    the first line starting COUNTRY=, its first two characters
  - verify: 7.258.0 a key line with no value no longer overrides an earlier
    assignment its parser skips (loader.conf, systemd, env, sysctl.d, bash)
  - verify: 7.258.0 a NetworkManager or BlueZ file GKeyFile rejects FAILs its
    rows: a stray line, a key before any [group], a bad group or key
  - verify: 7.258.0 a quoted loader.conf value is unquoted as systemd-boot
    reads it: double quotes, and single quotes from systemd 260 on
  - verify: 7.258.0 a comment ending in \ no longer swallows the next line: a
    ; line in systemd and env files, a # comment in sdboot-manage.conf
  - verify: 7.258.0 nftables.conf rows for flush ruleset, the inet filter
    table, invalid drop, established/related and loopback accept, all policies
  - verify: 7.258.0 a sudo lapse at a system file's readability or access
    probe WARNs as a lapse (check: exits 3), not a missing-file FAIL
  - verify: 7.258.0 one sudo lapse adds one WARN in the installed-file,
    parent-directory, NetworkManager, checksum, stray and boot-entry checks
  - verify: 7.258.0 a parent directory FAIL for a wrong owner or a group/other
    write names the chown or chmod go-w that fixes it
  - report: 7.247.0 an empty action list no longer claims a clean run the
    counts contradict; the UUID fallback calls an unreadable copy unreadable
  - report: 7.247.0 the counts ring follows the print palette; the sysctl
    coverage bar is named Kernel tunables
  - report: 7.249.0 a run failed only by generators no longer opens its
    verdict with '0 failed check(s)'
  - report: 7.254.0 a signal before the report tmpfile is tracked no longer
    strands it in the log tree
  - report: 7.254.0 - 7.255.0 a failed pacman -Q or unreadable /proc/cmdline
    grades its rows unreadable, as the ledger skips them
  - report: 7.255.0 a masked unit activating, reloading or refreshing grades
    masked, active, a MASK unit removed while running not installed, active
  - report: 7.255.0 the Root filesystem cell takes findmnt's first entry
  - report: 7.256.0 the embedded keys table lists WP_BLUEZ_ROLES
  - report: 7.257.0 a kernel token deployed but not live reads deployed, not
    live; pending reboot assumed the boot entries carry it
  - report: 7.257.0 a run stopped at the sudo gate counts and lists one FAIL;
    the gate's second line is now that row's hint
  - report: 7.258.0 an unresolvable $BOOT no longer adds an ERR action item
    under a passing verdict; the resolver's line is INFO
  - report: 7.258.0 C1 control characters become U+FFFD, as other controls do
  - report: 7.258.0 the appendix says a missing cmdline body can mean the root
    UUID is unresolved
  - logging: 7.247.0 a failed date leaves the header ts empty instead of
    breaking its JSON
  - logging: 7.250.0 closing log-file lines are skipped once stderr is gone;
    the preflight log is created as soon as its dir exists
  - logging: 7.251.0 the hardware gate logs HARDWARE_MATCH with the CPU;
    7.252.0 the root UUID gate logs ROOT_UUID_OK
  - logging: 7.254.0 preset _config values log MKINITCPIO_PRESET_CONFIG
  - logging: 7.255.0 VERIFY_STATIC_SUDO_LAPSE names probe=read or probe=size;
    a hardware refusal logs HARDWARE_*_REFUSED
  - logging: 7.257.0 BLUEZ_ROLES_RUNTIME headset= counts cards, not profile
    lines; a footer written on a signal counts every section's rows
  - logging: 7.257.0 a non-UTF-8 byte in a logged or reported string becomes
    U+FFFD, so the JSONL and the report stay valid UTF-8
  - logging: 7.258.0 VERIFY_RESULT names its scope; VERIFY_STATIC_MISMATCH for
    a post-NUL difference logs both byte counts, none if unreadable


7.235.0 - 7.245.0
-----------------

  - guard: 7.243.0 TMPDIR is pinned to /tmp; an inherited LOG_FILE or LOG_DIR
    is not deleted when the run exits before creating its own log
  - guard: 7.245.0 findmnt is checked at startup; a missing one exits 3
    instead of printing fish errors
  - signals: 7.245.0 a signal during the startup mv -T probe no longer leaves
    its temp files in the log tree
  - signals: 7.245.0 drop the PIPE handler; it silenced a live terminal
  - preflight: 7.240.0 iommu=off counts as no IOMMU for amdxdna
  - preflight: 7.243.0 a sudo -v that caches nothing (timestamp_timeout=0),
    7.245.0 or a passing sudo -n -v with sudo -n unusable, exits 3
  - preflight: 7.243.0 the dual-stack warning says to edit the nftables
    generator in both scripts
  - check: 7.243.0 --check stays silent when id(1) is missing; on a full disk
    it keeps fish's write errors off stderr
  - check: 7.243.0 a sudo lapse while sizing a file or probing nftables
    exits 3 instead of reporting drift
  - check: 7.245.0 the --check root guard reads one flag
  - verify: 7.235.0 the missing Vulkan package hint reads pacman -Syu --needed
    instead of a partial-upgrade -S
  - verify: 7.240.0 an ext4 row with nolazytime FAILs as a pending rewrite
  - verify: 7.241.0 the nftables policy-drop FAIL, 7.245.0 and the IPv4 ping
    WARN, name systemctl restart; sdboot-manage.conf keys check in order
  - verify: 7.243.0 mkinitcpio.conf.d drop-ins, dotfiles included, warn as
    overriding HOOKS and MODULES
  - verify: 7.243.0 a sudo lapse on mkinitcpio.conf warns once instead of
    failing every token; an unreadable loader entry is named unreadable
  - verify: 7.243.0 a failing NetworkManager --print-config (rc 7: a config NM
    cannot parse) FAILs; under a sudo lapse it warns
  - verify: 7.243.0 an ext4 fstab row with more than one commit= FAILs as a
    pending rewrite
  - verify: 7.243.0 ENV_VARS values systemctl prints $'...'-quoted are
    decoded; a failed show-environment warns
  - verify: 7.243.0 an active nftables unit whose live ruleset cannot be read
    warns; the IPv4 ping check matches the whole line
  - verify: 7.243.0 a sudo lapse during a checksum read or size probe warns
    instead of failing; the UUID fallback names its skip
  - verify: 7.243.0 a killed run's .ry-install.* temp file is listed as stray;
    stray and NM connection sweeps follow symlinked dirs
  - verify: 7.243.0 the failed user-unit count leaves out
    plasma-powerdevil.service, which has its own row
  - verify: 7.243.0 permission OK rows no longer print after a sudo lapse
    skipped files; the rule count reads unknown when nft did not run
  - verify: 7.243.0 a stop at the runtime gate keeps its rows in the totals
  - verify: 7.245.0 sysctl keys and values are trimmed as the generator writes
    them; the ICMPv6 check matches the whole rule
  - verify: 7.245.0 a symlinked loader/entries directory is listed; each
    missing Vulkan package gets its own install hint
  - verify: 7.245.0 a sudo lapse skips the HOOKS syntax check with one warning
    instead of a parse failure
  - verify: 7.245.0 a package file under a symlinked managed directory is
    matched by its resolved path, not listed as stray
  - verify: 7.245.0 a mismatch under the root-UUID fallback logs both SHA256s;
    the orphan-mask readlink test stays silent
  - report: 7.243.0 an action item carries only the marked hint row logged
    after it; a not-enabled nftables.service with a live ruleset grades WARN
  - report: 7.243.0 section 4 checks a symlink first and compares a file
    needing the unresolved root UUID under its installed copy's UUID
  - report: 7.243.0 unit and key names are HTML-escaped; a full disk reads as
    not written
  - report: 7.245.0 a failed show-environment marks the ENV_VARS rows
    unreadable and draws no coverage bar for them
  - report: 7.245.0 a 0600 /etc/kernel/cmdline is read with sudo -n; the
    System source list names /usr/lib/modules
  - report: 7.245.0 a size probe blocked by a sudo lapse grades no sudo
  - logging: 7.241.0 a signal between the log create and its header no longer
    leaves a footer-only JSONL; a child SIGKILL at cleanup is logged
  - logging: 7.243.0 a JSONL string ending in a newline keeps it; a run whose
    stderr write fails stops writing to the terminal
  - logging: 7.243.0 a removed log symlink is logged; no TMPFILE_STUCK line
    follows the footer
  - logging: 7.245.0 a failed stderr write logs STDERR_WRITE_FAIL once
  - logging: 7.245.0 tmpfile and child-reap records precede the footer; a
    signal while the footer is built still writes it
  - logging: 7.245.0 a log rename that falls back to cp, or fails, is logged;
    the log-write warning allows a removed JSONL
  - consistency: 7.242.0 error text reads 'unparsable', not 'unparseable'


7.217.0 - 7.234.0
-----------------

  - kernel: 7.217.0 drop ttm.pages_limit=20971520
  - perf: 7.217.0 governor performance -> powersave, EPP performance kept
  - env: 7.217.0 RADV_PERFTEST=nggc -> nggc,nircache; 7.224.0 expect
    SDL_GAMECONTROLLER_IGNORE_DEVICES
  - configuration: 7.229.0 MangoHud expects no_small_font and alpha=0.8, not
    text_outline; 7.232.0 text_outline=0
  - packages: 7.224.0 expect pipewire-jack installed
  - guard: 7.234.0 a sourced or piped run is refused under any locale; a
    startup signal exits 128+N silently; QUIT is no longer registered
  - read-only: 7.233.0 no mode creates ~/ry-install/backups, re-modes an
    existing ~/ry-install or writes to /tmp
  - read-only: 7.233.0 a group- or world-writable ~/ry-install exits 3
  - cli: 7.233.0 --h, --he, --hel and --vers, --versi, --versio are honored
    before the root guard
  - cli: 7.233.0 --help describes RY_INSTALL_SKIP_HARDWARE_CHECK accurately
  - cli: 7.234.0 -h and -v exit 1 on a full stdout (closed too, under fish 3);
    help alignment and the CPU-gate override hint match the code
  - cli: 7.234.0 --check stays silent on a non-numeric id -u; /tmp need not be
    writable
  - preflight: 7.233.0 a setgid $HOME or a symlinked ~/ry-install no longer
    fails the log-dir mode check
  - check: 7.233.0 a signal before argument parsing finishes no longer prints
    'Caught SIGTERM'; 7.234.0 --check keeps fish's log write errors off stderr
  - verify: 7.218.0 a sysctl or environment.d generator failure names entries
  - verify: 7.219.0 the nftables input-policy and IPv4-ping checks match their
    own rules; an unreadable expected unit warns instead of failing
  - verify: 7.224.0 WirePlumber rule checked in file and via pactl, P2P device
    unmanaged, more parser rejections caught, live fstab vs fstab options
  - verify: 7.229.0 drop the WirePlumber soft-mixer checks (file and pactl)
  - verify: 7.229.0 list strays beside managed files, package files by owner
  - verify: 7.230.0 stray-sibling match drops the ? wildcard; the stray OK row
    prints only after a full sweep
  - verify: 7.231.0 an unreadable sdboot-manage conf.d directory warns instead
    of reporting OK
  - verify: 7.233.0 startup WARNs count in the Combined line, the JSONL footer
    and the report verdict
  - verify: 7.233.0 the sdboot-manage drop-in check counts a symlinked *.conf,
    not a dotfile; a link only root can resolve warns when sudo has lapsed
  - verify: 7.233.0 KEY=value rows match the whole line;
    RADV_PERFTEST=nggc,nircache,sam no longer reads present
  - verify: 7.233.0 a managed file with bytes after a NUL reads MISMATCH and
    --check reports drift
  - verify: 7.233.0 a duplicate mkinitcpio HOOKS= line warns once per run
  - verify: 7.233.0 pacman -Qo and nmcli output is read under LC_ALL=C, so
    package files are not listed as Stray under a translated locale
  - verify: 7.233.0 the live IPv4-ping check needs an IPv4 icmp echo-request
    accept, not ICMPv6's; the policy-drop probe matches only the input hook
  - verify: 7.233.0 a CPU attribute no policy exposes reads as not exposed;
    uniform rows count the policies read
  - verify: 7.233.0 a symlinked parent dir of a managed file is checked at its
    target; loader.conf on an automounted ESP keeps its vfat perms skip
  - verify: 7.233.0 ESP autodetect reads the topmost non-autofs mount
  - verify: 7.233.0 no user bus: skipped ENV_VARS and user-unit checks warn
  - verify: 7.234.0 a token present only in a mkinitcpio.conf comment no
    longer reads as present; repeated BLS options lines combine
  - verify: 7.234.0 HOOKS presence uses the initcpio install dirs only
  - verify: 7.234.0 an active nftables unit FAILs without a live input policy
    drop; a failed nftables unit warns
  - verify: 7.234.0 sudo lapses read as WARN instead of FAIL
    (sdboot-manage.conf, file perms); a /boot symlink is refused on both paths
  - verify: 7.234.0 absent sysfs knobs are logged and unreadable ones warn
  - verify: 7.234.0 a digits-only fstab ext4 row warns once; stray files
    exclude exactly the backup names already reported
  - verify: 7.234.0 checksum rows say missing or unreadable; pacman -Qq
    failures log their rc
  - report: 7.233.0 a MASK unit that is not installed is neutral; the ESP row
    shows the ESP when bootctl -x resolved $BOOT
  - report: 7.234.0 an INFO note no longer attaches to an unrelated FAIL as
    its hint; no sudo grades WARN
  - report: 7.234.0 a failed generator shows no size or hash; a host without
    pacman renders cleanly


7.190.0 - 7.216.0
-----------------

  - boot: 7.211.0 the missing-entries hints drop --verbose
  - kernel: 7.195.0 fsck.mode=force; 7.199.0 add ttm.pages_limit=20971520;
    7.204.0 expect nowatchdog, read back through kernel.watchdog
  - perf: 7.204.0 expect governor powersave with EPP performance; 7.211.0
    expect governor performance again
  - env: 7.195.0 GSK_RENDERER ngl -> gl, drop PROTON_FSR4_INDICATOR=1; 7.212.1
    expect RADV_PERFTEST=nggc and MANGOHUD_DLSYM=1
  - configuration: 7.195.0 expect MangoHud cpu_stats enabled; 7.203.0 the
    managed-file header on every file but /etc/kernel/cmdline
  - configuration: 7.205.0 cpupower header names EnvironmentFile=; 7.211.0
    udev comments carry EPP and GPU levels; resolved drops mDNS/LLMNR claim
  - sysctl: 7.195.0 expect vm.watermark_scale_factor=125
  - verify: 7.195.1 module params derive from KERNEL_PARAMS; 7.197.0 exact
    integer compare; 7.198.0 read back every token; 7.200.0 connectivity off
  - verify: 7.202.0 - 7.207.0 a symlinked destination fails once at checksum,
    its perms row INFO; 7.206.0 bluetooth keys in emission order
  - verify: 7.207.0 a sudo bail closes its section; 7.203.0 - 7.208.0 labels
    read Wi-Fi and kernel.watchdog; 7.211.0 unset ENV_VARS name daemon-reload
  - verify: 7.211.0 the powerdevil crash hint matches coredumps by executable
    path; active but not-enabled units warn
  - check: 7.195.1 - 7.195.2 silence every bootstrap gate; 7.200.1 log
    CHECK_*_DRIFT with the cause; 7.202.0 record CHECK_SYMLINK_DRIFT
  - report: 7.214.0 --report runs --verify, then writes a self-contained HTML
    report (0600) beside the JSONL log; a failed write turns exit 0 into 1
  - report: 7.215.0 states grade as the ledger: not installed, running but not
    enabled, and still installed warn; the rest, a sudo lapse included, fail
  - report: 7.215.0 logs to report-*.jsonl; zero maximum clock, GPU IDs on an
    unreadable node, and a plain /boot repeating the / meter fixed
  - preflight: 7.211.0 a missing root UUID names findmnt's rc or empty output
  - cli: 7.211.0 glued short flags take only h and v; -Vh and -hV fail like
    -V; 7.215.0 --help names the report-not-written meaning of exit 1
  - logging: 7.211.0 a caught signal logs before its stderr line
  - split: 7.190.0 move ry-verify.fish here


7.139.0 - 7.189.0
-----------------

  - boot: COMPRESSION_OPTIONS -1 -> -3, drop -T0; fsck.mode=force -> auto
  - kernel: land on iommu=pt; drop amd_iommu, clearcpuid=umip, amdxdna
  - dns: drop pinned upstreams, DNSOverTLS= and DNSSEC=; link DNS wins
  - network: autoconnect-retries-default=0
  - env: PROTON_FSR4_UPGRADE -> FSR4_WATERMARK -> PROTON_FSR4_INDICATOR=1;
    expect GSK_RENDERER=ngl
  - configuration: assert the nftables ICMPv6 base accept
  - packages: 7.173.0 add cachyos-benchmarker
  - sysctl: drop both net.core.netdev_budget keys and vm.swappiness=150
  - backup: .ry.bak moves to ~/ry-install/backups; .ry.orig strays are INFO
  - verify: every cpufreq policy and non-fallback loader entries; resolved
    unit state, live ext4 opts, MangoHud, .ry.bak and parser complaints
  - check: mode drift sets drift; 7.186.1 - 7.187.0 take every --check
    abbreviation, silent rc 3
  - preflight: rc 3 on a reserved COUNTRY, NM_WIFI_POWERSAVE outside 0-3
  - split: 7.177.0 move verify and check to ry-verify.fish


7.100.0 - 7.138.0
-----------------

  - kernel: 7.123.0 - 7.129.0 add mt7925e.disable_aspm=1 and
    kernel.nmi_watchdog=0
  - dns: 7.123.0 - 7.129.0 pin upstreams in resolved and NM global-dns
  - env: 7.123.0 - 7.129.0 FSR4_UPGRADE -> PROTON_FSR4_UPGRADE; drop
    VKD3D_CONFIG
  - perf: 7.130.0 - 7.131.1 governor and EPP performance, GPU DPM level high
  - configuration: 7.137.0 - 7.138.0 drop the dormant RY_REMOTE_PLAY_PORTS
    nftables gate
  - services: 7.118.0 - 7.122.0 mask ufw instead of removing
  - verify: 7.100.0 - 7.107.3 SHA256 checksum, installed vs generated bytes
  - check: 7.135.0 - 7.136.1 record 60-ry-* drop-ins and masked units absent
    from MASK


7.99.1 and earlier
------------------

  - profile: initial profile for the Beelink GTR9 Pro
