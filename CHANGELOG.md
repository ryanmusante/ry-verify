Changes for ry-verify
=====================

Newest first. Versioning is MAJOR.MINOR.PATCH.

7.248.0
-------

  - env: expect 0x3434/0x0b00 (Keychron Q0 HE) in
    SDL_GAMECONTROLLER_IGNORE_DEVICES
  - configuration: udev expects the Xbox Wireless Controller libinput ignore


7.190.0 - 7.247.0
-----------------

  - kernel: 7.195.0 fsck.mode=force; 7.199.0 add ttm.pages_limit=20971520;
    7.204.0 expect nowatchdog, read back through kernel.watchdog
  - kernel: 7.217.0 drop ttm.pages_limit=20971520
  - perf: 7.204.0 expect governor powersave with EPP performance; 7.211.0
    governor performance again; 7.217.0 powersave, EPP performance kept
  - env: 7.195.0 GSK_RENDERER ngl -> gl, drop PROTON_FSR4_INDICATOR=1; 7.212.1
    expect RADV_PERFTEST=nggc and MANGOHUD_DLSYM=1
  - env: 7.217.0 RADV_PERFTEST=nggc -> nggc,nircache; 7.224.0 expect
    SDL_GAMECONTROLLER_IGNORE_DEVICES
  - sysctl: 7.195.0 expect vm.watermark_scale_factor=125
  - configuration: 7.195.0 expect MangoHud cpu_stats enabled; 7.203.0 the
    managed-file header on every file but /etc/kernel/cmdline
  - configuration: 7.205.0 cpupower header names EnvironmentFile=; 7.211.0
    udev comments carry EPP and GPU levels; resolved drops mDNS/LLMNR claim
  - configuration: 7.229.0 MangoHud expects no_small_font and alpha=0.8, not
    text_outline; 7.232.0 text_outline=0
  - packages: 7.224.0 expect pipewire-jack installed
  - boot: 7.211.0 the missing-entries hints drop --verbose
  - guard: 7.234.0 a sourced or piped run is refused under any locale; a
    startup signal exits 128+N silently; QUIT is no longer registered
  - guard: 7.243.0 an inherited LOG_FILE or LOG_DIR is not deleted when the
    run exits before creating its own log
  - guard: 7.243.0 TMPDIR is pinned to /tmp; 7.245.0 findmnt is checked at
    startup, a missing one exits 3 instead of printing fish errors
  - read-only: 7.233.0 no mode creates ~/ry-install/backups, re-modes an
    existing ~/ry-install or writes to /tmp
  - read-only: 7.233.0 a group- or world-writable ~/ry-install exits 3
  - preflight: 7.211.0 a missing root UUID names findmnt's rc or empty output
  - preflight: 7.233.0 a setgid $HOME or a symlinked ~/ry-install no longer
    fails the log-dir mode check
  - preflight: 7.240.0 iommu=off counts as no IOMMU for amdxdna; 7.247.0
    MKINITCPIO_MODULES and MKINITCPIO_HOOKS tokens are charset-checked
  - preflight: 7.243.0 a sudo -v that caches nothing (timestamp_timeout=0)
    exits 3 instead of failing later checks
  - preflight: 7.243.0 the dual-stack warning says to edit the nftables
    generator in both scripts
  - preflight: 7.245.0 a sudo -n -v that passes yet leaves sudo -n unusable
    exits 3
  - verify: 7.195.1 module params derive from KERNEL_PARAMS; 7.197.0 exact
    integer compare; 7.198.0 read back every token; 7.200.0 connectivity off
  - verify: 7.202.0 - 7.207.0 a symlinked destination fails once at checksum,
    its perms row INFO; 7.206.0 bluetooth keys in emission order
  - verify: 7.207.0 a sudo bail closes its section; 7.203.0 - 7.208.0 labels
    read Wi-Fi and kernel.watchdog; 7.211.0 unset ENV_VARS name daemon-reload
  - verify: 7.211.0 the powerdevil crash hint matches coredumps by executable
    path; active but not-enabled units warn
  - verify: 7.218.0 a sysctl or environment.d generator failure names entries;
    7.231.0 an unreadable sdboot-manage conf.d directory warns
  - verify: 7.219.0 the nftables input-policy and IPv4-ping checks match their
    own rules; an unreadable expected unit warns instead of failing
  - verify: 7.224.0 WirePlumber rule checked in file and via pactl, P2P device
    unmanaged, more parser rejections caught, live fstab vs fstab options
  - verify: 7.229.0 drop the WirePlumber soft-mixer checks (rule file and
    pactl)
  - verify: 7.229.0 list stray files beside managed files, package files by
    owner; 7.230.0 stray-sibling match drops the ? wildcard
  - verify: 7.230.0 the stray OK row prints only after a full sweep
  - verify: 7.233.0 startup WARNs count in the Combined line, the JSONL footer
    and the report verdict
  - verify: 7.233.0 the sdboot-manage drop-in check counts a symlinked *.conf,
    not a dotfile; a link only root can resolve warns when sudo has lapsed
  - verify: 7.233.0 KEY=value rows match the whole line;
    RADV_PERFTEST=nggc,nircache,sam no longer reads present
  - verify: 7.233.0 a managed file with bytes after a NUL reads MISMATCH and
    --check reports drift
  - verify: 7.233.0 a duplicate mkinitcpio HOOKS= line warns once per run;
    7.234.0 HOOKS presence uses the initcpio install dirs only
  - verify: 7.233.0 pacman -Qo and nmcli output is read under LC_ALL=C, so
    package files are not listed as Stray under a translated locale
  - verify: 7.233.0 the live IPv4-ping check needs an IPv4 icmp echo-request
    accept rule, not the ICMPv6 one
  - verify: 7.233.0 the policy-drop probe matches only the input hook line;
    ESP autodetect reads the topmost non-autofs mount
  - verify: 7.233.0 a CPU attribute no policy exposes reads as not exposed;
    uniform rows count the policies read
  - verify: 7.233.0 a symlinked parent dir of a managed file is checked at its
    target; loader.conf on an automounted ESP keeps its vfat perms skip
  - verify: 7.233.0 with no user bus, the skipped ENV_VARS and user-unit
    checks warn
  - verify: 7.234.0 a token present only in a mkinitcpio.conf comment no
    longer reads as present; repeated BLS options lines combine
  - verify: 7.234.0 an active nftables unit FAILs without a live input policy
    drop; a failed nftables unit warns
  - verify: 7.234.0 sudo lapses read as WARN instead of FAIL
    (sdboot-manage.conf, file perms); a /boot symlink is refused on both paths
  - verify: 7.234.0 absent sysfs knobs are logged and unreadable ones warn;
    checksum rows say missing or unreadable
  - verify: 7.234.0 a digits-only fstab ext4 row warns once; stray files
    exclude exactly the backup names already reported
  - verify: 7.234.0 pacman -Qq failures log their rc; 7.235.0 the missing
    Vulkan package hint reads pacman -Syu --needed
  - verify: 7.240.0 an ext4 fstab row carrying nolazytime, 7.243.0 or more
    than one commit=, FAILs as a pending rewrite
  - verify: 7.241.0 the nftables policy-drop FAIL names systemctl restart;
    7.245.0 so does the IPv4 ping WARN
  - verify: 7.241.0 sdboot-manage.conf keys check in emission order; 7.247.0
    the MASK state check is one pass
  - verify: 7.243.0 mkinitcpio.conf.d drop-ins, dotfiles included, warn as
    overriding HOOKS and MODULES
  - verify: 7.243.0 a sudo lapse on mkinitcpio.conf warns once instead of
    failing every token; an unreadable loader entry is named unreadable
  - verify: 7.243.0 a failing NetworkManager --print-config (rc 7: a config NM
    cannot parse) FAILs; under a sudo lapse it warns
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
  - verify: 7.243.0 a stop at the runtime gate keeps the gate rows in the
    totals
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
  - verify: 7.247.0 a running MASK unit that is not masked is named as such;
    the nftables policy-drop FAIL names the restart
  - verify: 7.247.0 efi and chainload loader entries are skipped; a stale
    60-ry-* drop-in under a symlinked modprobe.d is found
  - verify: 7.247.0 a system file behind a directory this user cannot traverse
    is checked with sudo; a dotfile read failure says to check owner and mode
  - verify: 7.247.0 a missing realpath no longer prints fish errors in the
    stray sweep
  - check: 7.195.1 - 7.195.2 silence every bootstrap gate; 7.200.1 log
    CHECK_*_DRIFT with the cause; 7.202.0 record CHECK_SYMLINK_DRIFT
  - check: 7.233.0 a signal before argument parsing finishes no longer prints
    'Caught SIGTERM' during --check
  - check: 7.234.0 --check keeps fish's own log write errors off stderr;
    7.243.0 it stays silent when id(1) is missing or the disk is full
  - check: 7.243.0 a sudo lapse while sizing a file or probing nftables exits
    3 instead of reporting drift
  - check: 7.245.0 the --check root guard reads one flag; 7.247.0 a masked
    MASK unit that is still running counts as drift
  - report: 7.214.0 --report runs --verify, then writes a self-contained HTML
    report (0600) beside the JSONL log; a failed write turns exit 0 into 1
  - report: 7.215.0 states grade as the ledger: not installed, running but not
    enabled, and still installed warn; the rest, a sudo lapse included, fail
  - report: 7.215.0 logs to report-*.jsonl; zero maximum clock, GPU IDs on an
    unreadable node, and a plain /boot repeating the / meter fixed
  - report: 7.233.0 a MASK unit that is not installed is neutral; the ESP row
    shows the ESP when bootctl -x resolved $BOOT
  - report: 7.234.0 an INFO note no longer attaches to an unrelated FAIL as
    its hint; no sudo grades WARN
  - report: 7.234.0 a failed generator shows no size or hash; a host without
    pacman renders cleanly
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
  - report: 7.245.0 a size probe blocked by a sudo lapse grades no sudo, not
    differs
  - report: 7.247.0 an empty action list no longer claims a clean run the
    counts contradict; the UUID fallback calls an unreadable copy unreadable
  - report: 7.247.0 the counts ring follows the print palette; the sysctl
    coverage bar is named Kernel tunables
  - cli: 7.211.0 glued short flags take only h and v; -Vh and -hV fail like
    -V; 7.215.0 --help names the report-not-written meaning of exit 1
  - cli: 7.233.0 --h, --he, --hel and --vers, --versi, --versio are honored
    before the root guard
  - cli: 7.233.0 --help describes RY_INSTALL_SKIP_HARDWARE_CHECK accurately;
    7.234.0 help alignment and the CPU-gate override hint match the code
  - cli: 7.234.0 -h and -v exit 1 when stdout is closed or full; --check stays
    silent on a non-numeric id -u; a writable /tmp is no longer required
  - signals: 7.245.0 a signal during the startup mv -T probe no longer leaves
    its temp files in the log tree
  - signals: 7.245.0 drop the PIPE handler; it silenced a live terminal
  - logging: 7.211.0 a caught signal logs before its stderr line
  - logging: 7.241.0 a signal between the log create and its header no longer
    leaves a footer-only JSONL; a child SIGKILL at cleanup is logged
  - logging: 7.243.0 a JSONL string ending in a newline keeps it; a run whose
    stderr write fails stops writing to the terminal
  - logging: 7.243.0 a removed log symlink is logged; no TMPFILE_STUCK line
    follows the footer
  - logging: 7.245.0 a failed stderr write logs STDERR_WRITE_FAIL once;
    7.247.0 the header timestamp survives a failed date
  - logging: 7.245.0 tmpfile and child-reap records precede the footer again;
    a signal while the footer is built still writes it
  - logging: 7.245.0 a log rename that falls back to cp, or fails, is logged;
    the log-write warning allows a removed JSONL
  - split: 7.190.0 move ry-verify.fish here
  - consistency: 7.242.0 error text reads 'unparsable', not 'unparseable'


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
  - dns: 7.123.0 - 7.129.0 pin upstreams in resolved and the NM global-dns
    section
  - env: 7.123.0 - 7.129.0 FSR4_UPGRADE -> PROTON_FSR4_UPGRADE; drop
    VKD3D_CONFIG
  - perf: 7.130.0 - 7.131.1 governor and EPP performance, GPU DPM level high
  - configuration: 7.137.0 - 7.138.0 drop the dormant RY_REMOTE_PLAY_PORTS
    nftables gate
  - services: 7.118.0 - 7.122.0 mask ufw instead of removing
  - verify: 7.100.0 - 7.107.3 SHA256 static checksum, installed vs generated
    bytes
  - check: 7.135.0 - 7.136.1 record 60-ry-* drop-ins and masked units absent
    from MASK


7.99.1 and earlier
------------------

  - initial profile for the Beelink GTR9 Pro
