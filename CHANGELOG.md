Changes for ry-verify
=====================

Newest first. Versioning is MAJOR.MINOR.PATCH.

7.300.0
-------

  - consistency: boot-entry hints drop their Run: label, as the other
    dash-joined fixes do
  - changelog: split the oversized newest range into four medium ranges and
    trim bullets


7.260.0 - 7.276.0
-----------------

  - guard: 7.260.0 fish_read_limit is pinned to 1 GiB; 7.264.0 a few-byte
    inherited one no longer breaks startup or a piped run's refusal
  - preflight: 7.269.0 a verifypw that lets sudo -v skip authentication
    prompts once through a command on a TTY instead of exiting 3
  - check: 7.260.0 an empty but readable /proc/cmdline is drift in every
    token; 7.267.0 a sudo lapse at the ruleset read exits 3, not 10
  - verify: 7.260.0 an ESP not at /boot, an XBOOTLDR $BOOT or a non-vfat
    fallback /boot FAILs the loader.conf section
  - verify: 7.260.0 an entry with no initrd line, a .. or a symlink out of
    $BOOT, or an ESP of only fallback entries FAILs
  - verify: 7.260.0 boot entries read as systemd-boot reads them; /boot is
    probed only as root; a read failing with sudo valid FAILs
  - verify: 7.260.0 an unlistable drop-in, preset, modprobe.d or connections
    dir names a sudo lapse or a read error
  - verify: 7.260.0 /etc/kernel/cmdline and mkinitcpio.conf tell an empty file
    or a lapse from a failed read; empty HOOKS=() FAILs
  - verify: 7.260.0 amd_pstate status is checked against KERNEL_PARAMS; owner
    names with a space pass, and the chown hint quotes them
  - verify: 7.266.0 processor.max_cstate is read back as root (0400 node);
    7.267.0 a lapse at the dmesg or ruleset read WARNs as a lapse
  - verify: 7.274.0 a GKeyFile key with a tab before its [locale] parses, as
    GLib loads it; an escaped trailing `\\` joins no line
  - verify: 7.274.0 the iw-regdomain FAIL names the two characters iw reads; a
    lapse in a permission loop says the rest went unchecked
  - report: 7.264.0 --report is gone (exit 2) and exit 1 no longer means an
    unwritten report
  - logging: 7.260.0 new VERIFY_ESP_LAYOUT_FAIL, VERIFY_STATIC_READ_FAIL and
    STATE_DIR_PERM_FIX keys; 7.265.0 every run logs KERNEL_RELEASE
  - consistency: 7.260.0 GKeyFile rows read parsable; 7.264.0 messages read
    user bus; udev rows read NVMe scheduler and GPU DPM
  - consistency: 7.273.0 - 7.275.0 messages drop sentence periods and
    abbreviations; refusals read refusing to; --help groups its sections
  - consistency: 7.276.0 the preset-directory probe quotes its test operands,
    as every other test does


7.256.0 - 7.258.0
-----------------

  - guard: 7.258.0 fish_read_limit is pinned to 100 MiB, so a config.fish or
    universal value no longer fails a large output
  - signals: 7.258.0 a signal while a section's counts are carried no longer
    counts those rows twice in the JSONL footer
  - preflight: 7.256.0 WP_BLUEZ_ROLES entries must be PipeWire role names;
    7.257.1 an EXPECTED_VULKAN_PKGS member in PKGS_DEL is refused
  - check: 7.257.0 CHECK_MODE_DRIFT names the mode as 0666; 7.258.0 a lapsed
    mode or owner probe exits 3, a running MASK unit with no file is drift
  - verify: 7.256.0 check the bluez5.roles drop-in and WARN on other config
    that sets it or overrides monitor.bluez.properties
  - verify: 7.256.0 WARN when a connected Bluetooth card offers an HFP/HSP
    profile (pactl); 7.257.0 an `hfp_*/hsp_*` role in WP_BLUEZ_ROLES skips it
  - verify: 7.257.0 root=UUID= must be a whole token, and an empty readable
    cmdline FAILs; a bracketed line no longer hides a later override
  - verify: 7.257.0 each linux entry's kernel and initrd must exist non-empty
    in $BOOT, 7.258.0 and not as a directory
  - verify: 7.257.0 a lapse at the HOOKS check WARNs per hook; an empty
    session variable FAILs; a multi-line HOOKS+=() or MODULES+=() appends
  - verify: 7.258.0 /etc/iw-regdomain is read as iw-set-regdomain reads it; a
    key with no value no longer overrides a skipped assignment
  - verify: 7.258.0 a NetworkManager or BlueZ file GKeyFile rejects FAILs its
    rows; loader.conf values unquote as systemd-boot does
  - verify: 7.258.0 udev lines ending in \ join and an unparsable rule drops;
    a comment ending in \ no longer swallows the next line
  - verify: 7.258.0 nftables.conf gains flush, table, drop, accept and policy
    rows; a set IgnorePkg reads INFO
  - verify: 7.258.0 a dangling /etc/fstab .ry.bak or .ry.orig link is a stray;
    a lapsed readability or access probe WARNs, once per check
  - logging: 7.257.0 BLUEZ_ROLES_RUNTIME headset= counts cards; a footer
    written on a signal counts every section; non-UTF-8 logs as U+FFFD
  - logging: 7.258.0 VERIFY_RESULT names its scope; a post-NUL
    VERIFY_STATIC_MISMATCH logs both byte counts


7.254.0 - 7.255.0
-----------------

  - cli: 7.254.0 argument errors and a root run's leftovers escape control
    characters, C1 and raw bytes; -h or -v ending in a newline exits 2
  - preflight: 7.254.0 a relative HOME, 7.255.0 or one with a control
    character, takes the passwd entry; such a passwd entry exits 3
  - preflight: 7.254.0 an embedded value ending in a newline is refused; the
    root UUID takes findmnt's first match under a stacked mount
  - preflight: 7.255.0 uncached sudo with no TTY prints one ERR; the --check
    override hint names the script as invoked
  - check: 7.255.0 a masked MASK unit activating, reloading or refreshing is
    drift; an expected unit reloading is not
  - check: 7.255.0 a unit no re-run installs reads as not installed, not
    drift; a system file not owned by root:root is drift
  - verify: 7.254.0 HOOKS, MODULES and COMPRESSION values match whole tokens,
    quotes stripped; a missing BINARIES= or FILES= line FAILs
  - verify: 7.254.0 a GPU DPM mismatch counts one FAIL per card, an unreadable
    level WARNs; bool params take any kstrtobool spelling
  - verify: 7.254.0 - 7.255.0 LINUX_OPTIONS reads as bash does (indented,
    single-quoted, last of repeats); unreadable cpufreq or amdgpu params WARN
  - verify: 7.254.0 - 7.255.0 a masked unit activating, reloading or
    refreshing FAILs as active; 7.255.0 a running one FAILs once, at runtime
  - verify: 7.254.0 - 7.255.0 fstab mount points decode and resolve as mount
    does; the live check reads a stacked target's first mount
  - verify: 7.255.0 key=value rows read keys as their parser does: last
    assignment, blanks at =, section, continued lines, quotes, sysctl forms
  - verify: 7.255.0 a HOOKS line's trailing backslash is no hook, and
    HOOKS+=() or MODULES+=() appends; MangoHud lines end at #
  - verify: 7.255.0 loader entries list as systemd-boot lists them; a $BOOT of
    only efi or chainload entries FAILs
  - verify: 7.255.0 IgnorePkg and ParallelDownloads come from pacman-conf
    (Include= counts); a MASK unit removed while running FAILs
  - verify: 7.255.0 the parameter scan reads dmesg when the journal lacks the
    boot lines, and WARNs when neither holds them
  - verify: 7.255.0 an unreadable /proc/cmdline WARNs once; a HOME under /etc
    or /boot is not read as system files
  - logging: 7.254.0 preset _config values log MKINITCPIO_PRESET_CONFIG;
    7.255.0 a hardware refusal logs HARDWARE_*_REFUSED


7.246.0 - 7.252.0
-----------------

  - env: 7.248.0 SDL_GAMECONTROLLER_IGNORE_DEVICES gains 0x3434/0x0b00 (Q0 HE)
  - configuration: 7.248.0 expect the Xbox Wireless Controller libinput
    ignore; 7.251.0 the 60-ry-modules.conf header claims no module options
  - guard: 7.250.0 inherited internal state (env, config.fish) is erased; a
    universal named like it, or regex-easyesc off, exits 3 (--check silent)
  - guard: 7.250.0 an early exit or a signal before the JSONL header removes
    only the empty dirs this run created and runs no user _do_cleanup
  - guard: 7.250.0 config.fish job control or IFS no longer steers a run;
    7.251.0 a universal _ROOT_UUID never steers the cmdline checks
  - signals: 7.250.0 a second startup signal no longer exits 0 or 1; one in an
    early exit's last step, 7.251.0 or before the main handler, keeps its code
  - preflight: 7.247.0 MODULES and HOOKS tokens are charset-checked; 7.252.0
    an empty or non-directory HOME names the value
  - check: 7.247.0 a masked MASK unit still running is drift; 7.250.0
    CHECK_UNIT_DRIFT names the expected state
  - check: 7.251.0 a RemainAfterExit nftables.service is probed for a live
    ruleset; 7.252.0 a disabled systemd-resolved is not drift
  - verify: 7.247.0 a running MASK unit not masked is named; the policy-drop
    FAIL names the restart; efi and chainload entries are skipped
  - verify: 7.247.0 a stale 60-ry-* drop-in under a symlinked modprobe.d is
    found; a file in a root-only directory is checked with sudo
  - verify: 7.249.0 CPU scaling driver, governor and EPP get one row each
    across all policies; 7.250.0 a failing one names the expected
  - verify: 7.250.0 the resolved check runs behind a root-only directory; a
    labeled MISSING row names the expected line
  - verify: 7.250.0 a preset _config is reported, a WARN when it names another
    file; 7.251.0 the preset scan skips empty or commented ones
  - verify: 7.250.0 a dangling .ry.bak or .ry.orig link is a stray; a
    directory at a .ry.bak name WARNs, 7.252.0 as does an empty .ry.bak
  - verify: 7.250.0 - 7.252.0 modprobe.d, initcpio/install, conf.d,
    mkinitcpio.d and NM dirs behind 0700 are read as root
  - verify: 7.252.0 an unlistable drop-in dir names the cause; a linux entry
    with no options line FAILs as missing every KERNEL_PARAMS token
  - logging: 7.247.0 a failed date leaves the header ts empty; 7.250.0 closing
    lines skip a gone stderr; the preflight log starts early
  - logging: 7.251.0 HARDWARE_MATCH logs the CPU; 7.252.0 the root UUID gate
    logs ROOT_UUID_OK


7.235.0 - 7.245.0
-----------------

  - guard: 7.243.0 TMPDIR is pinned to /tmp; 7.245.0 findmnt is checked at
    startup, and a missing one exits 3
  - signals: 7.245.0 a signal in the mv -T probe strands no temp files; drop
    the PIPE handler, which silenced a live terminal
  - preflight: 7.240.0 iommu=off counts as no IOMMU for amdxdna; 7.243.0 the
    dual-stack WARN names both nftables generators
  - preflight: 7.243.0 a sudo -v that caches nothing (timestamp_timeout=0),
    7.245.0 or a passing sudo -n -v with sudo -n unusable, exits 3
  - check: 7.243.0 --check stays silent when id(1) is missing or the disk is
    full; a lapse sizing a file or probing nftables exits 3
  - verify: 7.235.0 the missing Vulkan package hint reads
    pacman -Syu --needed; 7.245.0 each missing package gets its own hint
  - verify: 7.240.0 an ext4 row with nolazytime, 7.243.0 or several commit=,
    FAILs as a pending rewrite
  - verify: 7.241.0 the policy-drop FAIL, 7.245.0 and the IPv4 ping WARN, name
    systemctl restart; sdboot-manage.conf keys check in order
  - verify: 7.243.0 mkinitcpio.conf.d drop-ins and dotfiles warn as overrides;
    a lapse on mkinitcpio.conf warns once
  - verify: 7.243.0 an unreadable loader entry is named, an unreadable live
    ruleset warns; $'...'-quoted ENV_VARS are decoded
  - verify: 7.243.0 a lapsed checksum read or size probe warns; a killed run's
    temp file is a stray
  - verify: 7.243.0 stray and NM connection sweeps follow symlinked dirs; the
    failed user-unit count skips plasma-powerdevil
  - verify: 7.243.0 permission OK rows follow no lapse; a stop at the runtime
    gate keeps its rows in the totals
  - verify: 7.245.0 sysctl keys and values trim as the generator writes them;
    ping and ICMPv6 checks match whole lines
  - verify: 7.245.0 a symlinked loader/entries dir is listed; package files
    under a symlinked managed dir match by resolved path
  - logging: 7.241.0 a signal between the log create and its header leaves no
    footer-only JSONL; a child SIGKILL at cleanup is logged
  - logging: 7.243.0 a JSONL string keeps a trailing newline; a failed stderr
    write stops terminal output; removed log symlinks are logged
  - logging: 7.245.0 STDERR_WRITE_FAIL logs once; a signal while the footer is
    built still writes it; a cp-fallback log rename is logged
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
  - cli: 7.233.0 --h through --versio are honored before the root guard;
    7.234.0 -h and -v exit 1 on a full stdout (closed too, under fish 3)
  - preflight: 7.233.0 a setgid $HOME or a symlinked ~/ry-install no longer
    fails the log-dir mode check
  - check: 7.233.0 a signal before argument parsing prints no 'Caught
    SIGTERM'; 7.234.0 --check keeps fish's log write errors off stderr
  - verify: 7.218.0 a sysctl or environment.d generator failure names entries;
    7.219.0 an unreadable expected unit warns instead of failing
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
  - verify: 7.233.0 a duplicate HOOKS= line warns once; the ping check needs
    IPv4 echo-request
  - verify: 7.233.0 ESP autodetect reads the topmost non-autofs mount;
    symlinked parent dirs check at their target
  - verify: 7.233.0 no user bus warns for skipped checks; 7.234.0 a token only
    in a comment fails; repeated BLS options lines combine
  - verify: 7.234.0 HOOKS presence uses the install dirs only; an active
    nftables unit without a live policy drop FAILs, a failed one warns
  - verify: 7.234.0 sudo lapses read WARN, not FAIL; a /boot symlink is
    refused; a digits-only fstab ext4 row warns once


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
