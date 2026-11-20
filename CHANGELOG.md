Changes for ry-verify
=====================

Newest first. Versioning is MAJOR.MINOR.PATCH.

7.275.0
-------

  - consistency: the remaining refusals read refusing to run or emit
  - consistency: duplicate-key WARNs name the file, not conf; vfat skip notes
    pluralize by count, not file(s) and dir(s)


7.246.0 - 7.274.0
-----------------

  - env: 7.248.0 SDL_GAMECONTROLLER_IGNORE_DEVICES gains 0x3434/0x0b00 (Q0 HE)
  - configuration: 7.248.0 expect the Xbox Wireless Controller libinput
    ignore; 7.251.0 the 60-ry-modules.conf header claims no module options
  - configuration: 7.257.0 the expected drop-in comment no longer denies an
    `hfp_*/hsp_*` role
  - guard: 7.250.0 inherited internal state (env, config.fish) is erased; a
    universal named like it, or regex-easyesc off, exits 3 (--check silent)
  - guard: 7.250.0 an early exit or a signal before the JSONL header removes
    only the empty log and ~/ry-install dirs this run created
  - guard: 7.250.0 an early exit no longer runs a user's _do_cleanup, 7.251.0
    _dc_settle or _write_footer
  - guard: 7.250.0 config.fish job control or IFS no longer steers a run;
    fish_read_limit is pinned, 7.258.0 to 100 MiB, 7.260.0 to 1 GiB
  - guard: 7.250.0 a universal named like a tmpfile list or a boot-path,
    kernel-config or package cache is never read; _early_ch is not erased
  - guard: 7.251.0 a universal _ROOT_UUID never steers the cmdline checks
  - guard: 7.264.0 a few-byte inherited fish_read_limit no longer breaks
    startup or a piped run's refusal; a universal `_RPT_*` no longer exits 3
  - signals: 7.250.0 a second startup signal no longer exits 0 or 1; one in an
    early exit's last step, 7.251.0 or before the main handler, keeps its code
  - signals: 7.258.0 a signal while a section's counts are carried no longer
    counts those rows twice in the JSONL footer
  - cli: 7.252.0 an unexpected positional prints escaped, an empty one as '';
    7.254.0 a root run escapes each leftover argument whole
  - cli: 7.254.0 argument errors escape control characters, C1 and raw
    bytes; -h or -v ending in a newline is an unknown option (exit 2)
  - preflight: 7.247.0 MODULES and HOOKS tokens are charset-checked; 7.252.0
    an empty or non-directory HOME names the value
  - preflight: 7.252.0 the CPUPOWER_GOVERNOR refusal names the ry-install
    check; 7.254.0 an embedded value ending in a newline is refused
  - preflight: 7.254.0 a relative HOME, 7.255.0 or one with a control
    character, takes the passwd entry; such a passwd entry exits 3
  - preflight: 7.254.0 the root UUID takes findmnt's first match under a
    stacked mount; 7.255.0 uncached sudo with no TTY prints one ERR
  - preflight: 7.255.0 the --check override hint names the script as invoked;
    7.256.0 WP_BLUEZ_ROLES entries must be PipeWire role names
  - preflight: 7.257.1 an EXPECTED_VULKAN_PKGS member in PKGS_DEL or starting
    with a dash is refused; 7.258.0 true(1) and test(1) are checked
  - preflight: 7.269.0 a verifypw that lets sudo -v skip authentication
    prompts once through a command on a TTY instead of exiting 3
  - check: 7.247.0 a masked MASK unit still running is drift, 7.255.0 as is
    one activating, reloading or refreshing; an expected unit reloading is not
  - check: 7.250.0 CHECK_UNIT_DRIFT names the expected state, 7.251.0
    load=not-found expected=loaded for a missing unit
  - check: 7.250.0 a root-only managed symlink no longer records its mode as
    drift
  - check: 7.251.0 nftables.service is probed for a live ruleset even when a
    RemainAfterExit drop-in keeps the unit active
  - check: 7.252.0 a disabled systemd-resolved logs CONFD_UNIT_NOT_ENABLED,
    not drift; with no sudo binary an unlistable modprobe.d stays silent
  - check: 7.255.0 a unit no re-run installs reads as not installed, not
    drift; a system file not owned by root:root is drift
  - check: 7.257.0 CHECK_MODE_DRIFT names the mode as 0666; 7.258.0 a post-NUL
    difference logs reason=size; a lapsed mode or owner probe exits 3
  - check: 7.258.0 a MASK unit whose file is gone but still runs is drift;
    7.260.0 an empty but readable /proc/cmdline is drift in every token
  - check: 7.267.0 a sudo lapse at the nftables ruleset read exits 3, not 10
  - verify: 7.247.0 a running MASK unit not masked is named; the policy-drop
    FAIL names the restart; efi and chainload entries are skipped
  - verify: 7.247.0 a stale 60-ry-* drop-in under a symlinked modprobe.d is
    found; a file in a root-only directory is checked with sudo
  - verify: 7.247.0 a dotfile read failure says to check owner and mode; the
    stray sweep prints no fish error without realpath
  - verify: 7.249.0 CPU scaling driver, governor and EPP get one row each
    across all policies; 7.250.0 a failing one names the expected
  - verify: 7.250.0 the resolved check runs behind a root-only directory;
    symlinked mkinitcpio.conf.d drop-ins no longer warn
  - verify: 7.250.0 a preset _config is reported, a WARN when it names another
    file; 7.251.0 the preset scan skips empty or commented ones
  - verify: 7.250.0 a dangling link at a managed file's .ry.bak or .ry.orig
    name is a stray, 7.258.0 as is one at /etc/fstab's
  - verify: 7.250.0 a directory at a .ry.bak name WARNs, 7.251.0 only for the
    files ry-install backs up; 7.252.0 an empty .ry.bak likewise
  - verify: 7.250.0 a stale 60-ry-* drop-in behind a 0700 /etc/modprobe.d is
    reported; NM keyfile modes are checked behind a 0700 parent
  - verify: 7.250.0 a HOOKS build script behind a 0700 /etc/initcpio/install
    no longer reads missing; 7.251.0 sudo probes only unseen ones
  - verify: 7.250.0 a labeled MISSING row names the expected line; 7.251.0
    0700 conf.d dirs list as root, and a lapsed 0700 NM dir WARNs
  - verify: 7.252.0 presets in a 0700 /etc/mkinitcpio.d or 0600 files are read
    as root; a config several presets name is listed once
  - verify: 7.252.0 an unlistable drop-in dir names the cause; a linux entry
    with no options line FAILs as missing every KERNEL_PARAMS token
  - verify: 7.254.0 x-only preset dirs read as root, hidden presets skipped;
    HOOKS, MODULES and COMPRESSION values match whole tokens
  - verify: 7.254.0 the hook check strips quotes and reads HOOKS without
    parentheses; a missing BINARIES= or FILES= line FAILs
  - verify: 7.254.0 a whole-line check FAILs a trailing # its parser keeps; an
    array open at EOF reads empty; an unlistable modprobe.d WARNs
  - verify: 7.254.0 a GPU DPM mismatch counts one FAIL per card, an unreadable
    level WARNs; bool params take any kstrtobool spelling
  - verify: 7.254.0 - 7.255.0 LINUX_OPTIONS reads as bash does (indented,
    single-quoted, last of repeats); unreadable cpufreq or amdgpu params WARN
  - verify: 7.254.0 - 7.255.0 a masked unit activating, reloading or
    refreshing FAILs as active; 7.255.0 a running one FAILs once, at runtime
  - verify: 7.254.0 - 7.255.0 fstab mount points decode and resolve as mount
    does: \040, slashes, backslashes, symlinks and .. hide no mount
  - verify: 7.255.0 a udev rule with a trailing # reads as dropped, MangoHud
    lines end at #; 7.258.0 \ lines join and an unparsable rule drops
  - verify: 7.255.0 key=value rows read keys as their parser does: last
    assignment, blanks at =, section, continued lines, quotes, sysctl forms
  - verify: 7.255.0 a HOOKS line's trailing backslash is no hook, and
    HOOKS+=() or MODULES+=() appends, 7.257.0 even across lines
  - verify: 7.255.0 the live fstab check reads a stacked target's first mount
    and needs a managed option to compare; a lapse is named
  - verify: 7.255.0 loader entries list as systemd-boot lists them; a $BOOT of
    only efi or chainload entries FAILs; an unresolvable one counts once
  - verify: 7.255.0 IgnorePkg and ParallelDownloads come from pacman-conf
    (Include= counts); 7.258.0 a set IgnorePkg reads INFO
  - verify: 7.255.0 a MASK unit removed while running FAILs as running; an
    expected unit reloading or refreshing counts as active
  - verify: 7.255.0 the parameter scan reads dmesg when the journal lacks the
    boot lines, and WARNs when neither holds them
  - verify: 7.255.0 a failed iw reg get reads unqueryable; NetworkManager down
    reads nmcli failed; an unreadable /proc/cmdline WARNs once
  - verify: 7.255.0 names list NUL-safe with control characters as ?; a HOME
    under /etc or /boot is not read as system files
  - verify: 7.255.0 the NetworkManager connection count skips the files
    keyfile ignores (hidden, ~ backups, temp, .pem, .der)
  - verify: 7.256.0 check the bluez5.roles drop-in and WARN on other config
    that sets it or overrides monitor.bluez.properties
  - verify: 7.256.0 WARN when a connected Bluetooth card offers an HFP/HSP
    profile or its profile list is unreadable (pactl); no user bus WARNs
  - verify: 7.257.0 root=UUID= must be a whole token, and an empty readable
    cmdline FAILs; a bracketed line no longer hides a later override
  - verify: 7.257.0 an `hfp_*/hsp_*` role in WP_BLUEZ_ROLES skips the headset
    check; a lapse adds no 'No installed files' WARN
  - verify: 7.257.0 each linux entry's kernel and initrd must exist non-empty
    in $BOOT, 7.258.0 and not as a directory
  - verify: 7.257.0 a lapse at the HOOKS check WARNs per hook, at
    ParallelDownloads is named; an empty session variable FAILs as differing
  - verify: 7.257.0 a failed fstab scan WARNs in both fstab checks; a blank
    ufw state reads unknown
  - verify: 7.258.0 /etc/iw-regdomain is read as iw-set-regdomain reads it; a
    key with no value no longer overrides a skipped assignment
  - verify: 7.258.0 a NetworkManager or BlueZ file GKeyFile rejects FAILs its
    rows; loader.conf values unquote as systemd-boot does
  - verify: 7.258.0 a comment ending in \ no longer swallows the next line;
    nftables.conf gains flush, table, drop, accept and policy rows
  - verify: 7.258.0 a lapsed readability or access probe WARNs; one lapse adds
    one WARN per check; parent directory FAILs name the chown or chmod fix
  - verify: 7.260.0 an ESP not at /boot, an XBOOTLDR $BOOT or a non-vfat
    fallback /boot FAILs the loader.conf section
  - verify: 7.260.0 an entry with no initrd line, a .. or a symlink out of
    $BOOT, or an ESP of only fallback entries FAILs
  - verify: 7.260.0 boot entries read as systemd-boot reads them; /boot is
    probed only as root; a read failing with sudo valid FAILs
  - verify: 7.260.0 an unlistable drop-in, preset, modprobe.d or connections
    dir names a sudo lapse or a read error
  - verify: 7.260.0 /etc/kernel/cmdline tells empty from unreadable, both FAIL
    without a root UUID; the amdgpu row follows MKINITCPIO_MODULES
  - verify: 7.260.0 mkinitcpio.conf tells a lapse from a failed read or
    missing key; empty HOOKS=() and header-only files FAIL
  - verify: 7.260.0 owner names with a space pass, and the chown hint quotes
    them; a raw split_lock_detect= is INFO on older kernels
  - verify: 7.260.0 amd_pstate status is checked against KERNEL_PARAMS; a
    REJECTED or raw token gets no second WARN
  - verify: 7.266.0 processor.max_cstate is read back as root (0400 node);
    7.267.0 a lapse at the dmesg or ruleset read WARNs as a lapse
  - verify: 7.274.0 a GKeyFile key with a tab before its [locale] parses, as
    GLib loads it; an escaped trailing `\\` joins no line
  - verify: 7.274.0 the iw-regdomain FAIL names the two characters iw reads; a
    running MASK unit offers re-running ry-install first
  - verify: 7.274.0 a sudo lapse in the file, directory and connection
    permission loops says the rest went unchecked
  - report: 7.247.0 - 7.260.0 HTML report fixes; 7.264.0 --report is gone
    (exit 2) and exit 1 no longer means an unwritten report
  - logging: 7.247.0 a failed date leaves the header ts empty; 7.250.0 closing
    lines skip a gone stderr; the preflight log starts early
  - logging: 7.251.0 HARDWARE_MATCH logs the CPU; 7.252.0 the root UUID gate
    logs ROOT_UUID_OK; 7.254.0 MKINITCPIO_PRESET_CONFIG
  - logging: 7.255.0 VERIFY_STATIC_SUDO_LAPSE names probe=read or probe=size;
    a hardware refusal logs HARDWARE_*_REFUSED
  - logging: 7.257.0 BLUEZ_ROLES_RUNTIME headset= counts cards; a footer
    written on a signal counts every section; non-UTF-8 logs as U+FFFD
  - logging: 7.258.0 VERIFY_RESULT names its scope; VERIFY_STATIC_MISMATCH for
    a post-NUL difference logs both byte counts
  - logging: 7.260.0 new RESOLVED_RUNTIME_SKIP, KPARAM_RAW_READ,
    VERIFY_ESP_LAYOUT_FAIL, VERIFY_STATIC_READ_FAIL, STATE_DIR_PERM_FIX keys
  - logging: 7.264.0 CHECK_UNIT_NOT_INSTALLED says --verify WARNs, not the
    ledger; 7.265.0 every run logs KERNEL_RELEASE
  - consistency: 7.260.0 GKeyFile rows read parsable; 7.264.0 messages read
    user bus; udev rows read NVMe scheduler and GPU DPM
  - consistency: 7.273.0 --help and refusals drop sentence periods; lists take
    a serial comma; repeated keys read 'using the last'
  - consistency: 7.274.0 sysfs FAILs name the expected value; hints join fixes
    with a dash; --help groups its sections; abbreviations spelled out


7.235.0 - 7.245.0
-----------------

  - guard: 7.243.0 TMPDIR is pinned to /tmp; an inherited LOG_FILE or LOG_DIR
    is not deleted when the run exits before its own log
  - guard: 7.245.0 findmnt is checked at startup; a missing one exits 3
  - signals: 7.245.0 a signal in the mv -T probe strands no temp files; drop
    the PIPE handler, which silenced a live terminal
  - preflight: 7.240.0 iommu=off counts as no IOMMU for amdxdna; 7.243.0 the
    dual-stack WARN names both nftables generators
  - preflight: 7.243.0 a sudo -v that caches nothing (timestamp_timeout=0),
    7.245.0 or a passing sudo -n -v with sudo -n unusable, exits 3
  - check: 7.243.0 --check stays silent when id(1) is missing or the disk is
    full; a lapse sizing a file or probing nftables exits 3
  - check: 7.245.0 the --check root guard reads one flag
  - verify: 7.235.0 the missing Vulkan package hint reads
    pacman -Syu --needed; 7.245.0 each missing package gets its own hint
  - verify: 7.240.0 an ext4 row with nolazytime, 7.243.0 or several commit=,
    FAILs as a pending rewrite
  - verify: 7.241.0 the policy-drop FAIL, 7.245.0 and the IPv4 ping WARN, name
    systemctl restart; sdboot-manage.conf keys check in order
  - verify: 7.243.0 mkinitcpio.conf.d drop-ins and dotfiles warn as overrides;
    a lapse on mkinitcpio.conf warns once
  - verify: 7.243.0 an unreadable loader entry is named; a failing
    NetworkManager --print-config FAILs, under a lapse warns
  - verify: 7.243.0 $'...'-quoted ENV_VARS are decoded and a failed
    show-environment warns; an unreadable live ruleset warns
  - verify: 7.243.0 a lapsed checksum read or size probe warns; the UUID
    fallback names its skip; a killed run's temp file is a stray
  - verify: 7.243.0 stray and NM connection sweeps follow symlinked dirs; the
    failed user-unit count skips plasma-powerdevil
  - verify: 7.243.0 permission OK rows follow no lapse; nft rule counts read
    unknown when nft did not run
  - verify: 7.243.0 a stop at the runtime gate keeps its rows in the totals
  - verify: 7.245.0 sysctl keys and values trim as the generator writes them;
    ping and ICMPv6 checks match whole lines
  - verify: 7.245.0 a symlinked loader/entries dir is listed; a lapse skips
    the HOOKS check with one warning
  - verify: 7.245.0 package files under a symlinked managed dir match by
    resolved path; a fallback mismatch logs both SHA-256s
  - report: 7.243.0 - 7.245.0 HTML report fixes
  - logging: 7.241.0 a signal between the log create and its header leaves no
    footer-only JSONL; a child SIGKILL at cleanup is logged
  - logging: 7.243.0 a JSONL string keeps a trailing newline; a failed stderr
    write stops terminal output; removed log symlinks are logged
  - logging: 7.243.0 no TMPFILE_STUCK follows the footer; 7.245.0
    STDERR_WRITE_FAIL logs once; tmpfile and reap records precede the footer
  - logging: 7.245.0 a signal while the footer is built still writes it; a log
    rename that falls back to cp, or fails, is logged
  - consistency: 7.242.0 error text reads 'unparsable', not 'unparseable'


7.217.0 - 7.234.0
-----------------

  - kernel: 7.217.0 drop ttm.pages_limit=20971520
  - perf: 7.217.0 governor performance -> powersave, EPP performance kept
  - env: 7.217.0 RADV_PERFTEST=nggc -> nggc,nircache; 7.220.0 expect
    SDL_GAMECONTROLLER_IGNORE_DEVICES
  - configuration: 7.229.0 MangoHud expects no_small_font and alpha=0.8, not
    text_outline; 7.232.0 text_outline=0
  - packages: 7.220.0 expect pipewire-jack installed
  - guard: 7.234.0 a sourced or piped run is refused under any locale; a
    startup signal exits 128+N silently; QUIT is no longer registered
  - read-only: 7.233.0 no mode creates ~/ry-install/backups, chmods
    ~/ry-install or writes to /tmp; a group- or world-writable one exits 3
  - cli: 7.233.0 --h, --he, --hel and --vers through --versio are honored
    before the root guard; --help describes the hardware override
  - cli: 7.234.0 -h and -v exit 1 on a full stdout (closed too, under fish
    3); --check stays silent on a bad id -u; /tmp need not be writable
  - preflight: 7.233.0 a setgid $HOME or a symlinked ~/ry-install no longer
    fails the log-dir mode check
  - check: 7.233.0 a signal before argument parsing prints no 'Caught
    SIGTERM'; 7.234.0 --check keeps fish's log write errors off stderr
  - verify: 7.218.0 a sysctl or environment.d generator failure names entries;
    7.219.0 nftables policy and ping checks match their own rules
  - verify: 7.219.0 an unreadable expected unit warns instead of failing
  - verify: 7.220.0 the WirePlumber rule is checked in file and via pactl, the
    P2P device unmanaged; 7.224.0 more parser rejections, live fstab
  - verify: 7.229.0 drop the WirePlumber soft-mixer checks; list strays beside
    managed files, package files by owner
  - verify: 7.230.0 stray-sibling match drops the ? wildcard, its OK row
    prints after a full sweep; 7.231.0 an unreadable sdboot conf.d warns
  - verify: 7.233.0 startup WARNs count in the Combined line and footer; a
    symlinked *.conf drop-in counts, a dotfile does not
  - verify: 7.233.0 KEY=value rows match the whole line
    (RADV_PERFTEST=nggc,nircache,sam fails); bytes after a NUL read MISMATCH
  - verify: 7.233.0 a duplicate HOOKS= line warns once; pacman -Qo and nmcli
    read under LC_ALL=C; the ping check needs IPv4 echo-request
  - verify: 7.233.0 a CPU attribute no policy exposes reads not exposed;
    symlinked parent dirs check at their target
  - verify: 7.233.0 ESP autodetect reads the topmost non-autofs mount
  - verify: 7.233.0 no user bus warns for skipped checks; 7.234.0 a token only
    in a comment fails; repeated BLS options lines combine
  - verify: 7.234.0 HOOKS presence uses the install dirs only; an active
    nftables unit without a live policy drop FAILs, a failed one warns
  - verify: 7.234.0 sudo lapses read WARN, not FAIL; a /boot symlink is
    refused; unreadable sysfs knobs warn; checksum rows say why
  - verify: 7.234.0 a digits-only fstab ext4 row warns once; strays exclude
    the backup names already reported
  - report: 7.233.0 - 7.234.0 HTML report grading fixes


7.190.0 - 7.216.0
-----------------

  - kernel: 7.195.0 fsck.mode=force; 7.199.0 add ttm.pages_limit=20971520;
    7.204.0 expect nowatchdog, read back through kernel.watchdog
  - perf: 7.204.0 expect governor powersave with EPP performance; 7.211.0
    expect governor performance again
  - env: 7.195.0 GSK_RENDERER ngl -> gl, drop PROTON_FSR4_INDICATOR=1; 7.212.1
    expect RADV_PERFTEST=nggc and MANGOHUD_DLSYM=1
  - sysctl: 7.195.0 expect vm.watermark_scale_factor=125
  - configuration: 7.195.0 expect MangoHud cpu_stats enabled; 7.203.0 the
    managed-file header on every file but /etc/kernel/cmdline
  - configuration: 7.205.0 cpupower header names EnvironmentFile=; 7.211.0
    udev comments carry EPP and GPU levels; resolved drops mDNS/LLMNR claim
  - cli: 7.211.0 glued short flags take only h and v; -Vh and -hV fail like -V
  - preflight: 7.211.0 a missing root UUID names findmnt's rc or empty output
  - boot: 7.211.0 the missing-entries hints drop --verbose
  - check: 7.195.1 - 7.195.2 silence every bootstrap gate; 7.200.1 log
    CHECK_*_DRIFT with the cause; 7.202.0 record CHECK_SYMLINK_DRIFT
  - verify: 7.195.1 module params derive from KERNEL_PARAMS; 7.197.0 exact
    integer compare; 7.198.0 read back every token; 7.200.0 connectivity off
  - verify: 7.202.0 - 7.207.0 a symlinked destination fails once at checksum,
    its perms row INFO; 7.206.0 bluetooth keys in emission order
  - verify: 7.203.0 - 7.208.0 labels read Wi-Fi and kernel.watchdog; 7.207.0 a
    sudo bail closes its section; 7.211.0 unset ENV_VARS name daemon-reload
  - verify: 7.211.0 the powerdevil crash hint matches coredumps by executable
    path; active but not-enabled units warn
  - report: 7.214.0 --report runs --verify and writes an HTML report (0600)
    beside the JSONL log; 7.215.0 states grade as the ledger
  - logging: 7.211.0 a caught signal logs before its stderr line
  - split: 7.190.0 move ry-verify.fish here


7.139.0 - 7.189.0
-----------------

  - kernel: 7.160.0 drop clearcpuid=umip; 7.162.0 land on iommu=pt, drop the
    amdxdna blacklist; 7.164.0 drop amd_iommu
  - network: 7.163.0 autoconnect-retries-default=0
  - dns: 7.148.0 drop pinned upstreams, DNSOverTLS= and DNSSEC=; link DNS wins
  - env: 7.154.0 PROTON_FSR4_UPGRADE -> FSR4_WATERMARK; 7.163.0 expect
    GSK_RENDERER=ngl; 7.181.0 FSR4_WATERMARK -> PROTON_FSR4_INDICATOR=1
  - sysctl: 7.157.0 drop both net.core.netdev_budget keys; 7.171.0 drop
    vm.swappiness=150
  - configuration: 7.159.0 assert the nftables ICMPv6 base accept
  - packages: 7.173.0 add cachyos-benchmarker
  - preflight: 7.185.1 an NM_WIFI_POWERSAVE outside 0-3 exits 3, as a reserved
    COUNTRY does
  - backup: 7.174.0 .ry.bak moves to ~/ry-install/backups; 7.176.0 .ry.orig
    strays are INFO
  - boot: 7.140.0 drop -T0; 7.158.0 COMPRESSION_OPTIONS -1 -> -3,
    fsck.mode=force -> auto
  - check: 7.171.0 mode drift sets drift; 7.186.1 - 7.187.0 take every --check
    abbreviation, silent rc 3
  - verify: 7.139.0 every cpufreq policy, non-fallback loader entries and
    resolved unit state
  - verify: 7.165.0 live ext4 opts, .ry.bak and parser complaints; 7.180.0
    MangoHud
  - split: 7.177.0 move verify and check to ry-verify.fish


7.100.0 - 7.138.0
-----------------

  - kernel: 7.123.0 - 7.129.0 add mt7925e.disable_aspm=1 and
    kernel.nmi_watchdog=0
  - perf: 7.130.0 - 7.131.1 governor and EPP performance, GPU DPM level high
  - dns: 7.123.0 - 7.129.0 pin upstreams in resolved and NM global-dns
  - env: 7.123.0 - 7.129.0 FSR4_UPGRADE -> PROTON_FSR4_UPGRADE; drop
    VKD3D_CONFIG
  - configuration: 7.137.0 - 7.138.0 drop the dormant RY_REMOTE_PLAY_PORTS
    nftables gate
  - services: 7.118.0 - 7.122.0 mask ufw instead of removing
  - check: 7.135.0 - 7.136.1 record 60-ry-* drop-ins, masks not in MASK
  - verify: 7.100.0 - 7.107.3 SHA-256 checksum, installed vs generated bytes


7.99.1 and earlier
------------------

  - profile: initial profile for the Beelink GTR9 Pro
