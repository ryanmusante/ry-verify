Changes for ry-verify
=====================

Newest first. Versioning is MAJOR.MINOR.PATCH.

7.240.0
-------

  - verify: an ext4 fstab row carrying nolazytime FAILs as a pending rewrite,
    as ry-install now strips it
  - preflight: iommu=off counts as no IOMMU for amdxdna, as in ry-install


7.235.0
-------

  - verify: the missing Vulkan package hint reads pacman -Syu --needed instead
    of a partial-upgrade -S


7.234.0
-------

  - guard: a sourced or piped run is refused under any locale; a startup
    signal exits 128+N silently; QUIT is no longer registered
  - cli: -h and -v exit 1 when stdout is closed or full; --check stays
    silent on a non-numeric id -u; a writable /tmp is no longer required
  - check: --check keeps fish's own log write errors off stderr
  - verify: a token present only in a mkinitcpio.conf comment no longer reads
    as present; repeated BLS options lines combine
  - verify: HOOKS presence uses the initcpio install dirs only
  - verify: an active nftables unit FAILs without a live input policy drop; a
    failed nftables unit warns
  - verify: sudo lapses read as WARN instead of FAIL (sdboot-manage.conf, file
    perms); a /boot symlink is refused on both paths
  - verify: absent sysfs knobs are logged and unreadable ones warn
  - verify: a digits-only fstab ext4 row warns once; stray files exclude
    exactly the backup names already reported
  - verify: checksum rows say missing or unreadable; pacman -Qq failures log
    their rc
  - report: an INFO note no longer attaches to an unrelated FAIL as its hint;
    no sudo grades WARN
  - report: a failed generator shows no size or hash; a host without pacman
    renders cleanly
  - consistency: function descriptions, banners, help alignment and the
    CPU-gate override hint match the code; lines over 300 characters are split


7.233.0
-------

  - verify: startup WARNs count in the Combined line, the JSONL footer and the
    report verdict
  - verify: the sdboot-manage drop-in check counts a symlinked *.conf, not a
    dotfile; a link only root can resolve warns when sudo has lapsed
  - verify: KEY=value rows match the whole line;
    RADV_PERFTEST=nggc,nircache,sam no longer reads present
  - verify: a managed file with bytes after a NUL reads MISMATCH and --check
    reports drift
  - verify: a duplicate mkinitcpio HOOKS= line warns once per run
  - verify: pacman -Qo and nmcli output is read under LC_ALL=C, so package
    files are not listed as Stray under a translated locale
  - verify: the live IPv4-ping check needs an IPv4 icmp echo-request accept
    rule, not the ICMPv6 one
  - verify: the policy-drop probe matches only the input hook line
  - verify: a CPU attribute no policy exposes reads as not exposed; uniform
    rows count the policies read
  - verify: a symlinked parent dir of a managed file is checked at its
    target; loader.conf on an automounted ESP keeps its vfat perms skip
  - verify: ESP autodetect reads the topmost non-autofs mount
  - verify: with no user bus, the skipped ENV_VARS and user-unit checks warn
  - report: a MASK unit that is not installed is neutral; the ESP row shows
    the ESP when bootctl -x resolved $BOOT
  - read-only: no mode creates ~/ry-install/backups, re-modes an existing
    ~/ry-install or writes to /tmp
  - read-only: a group- or world-writable ~/ry-install exits 3
  - preflight: a setgid $HOME or a symlinked ~/ry-install no longer fails
    the log-dir mode check
  - cli: --h, --he, --hel and --vers, --versi, --versio are honored before the
    root guard
  - cli: --help describes RY_INSTALL_SKIP_HARDWARE_CHECK accurately
  - check: a signal before argument parsing finishes no longer prints
    'Caught SIGTERM' during --check


7.232.0
-------

  - configuration: MangoHud expects text_outline=0


7.217.0 - 7.231.0
-----------------

  - kernel: 7.217.0 drop ttm.pages_limit=20971520
  - perf: 7.217.0 governor performance -> powersave, EPP performance kept
  - env: 7.217.0 RADV_PERFTEST=nggc -> nggc,nircache; 7.224.0 expect
    SDL_GAMECONTROLLER_IGNORE_DEVICES
  - configuration: 7.229.0 MangoHud expects no_small_font and alpha=0.8, not
    text_outline
  - packages: 7.224.0 expect pipewire-jack installed
  - verify: 7.218.0 a sysctl or environment.d generator failure names entries
  - verify: 7.219.0 the nftables input-policy and IPv4-ping checks match their
    own rules; an unreadable expected unit warns instead of failing
  - verify: 7.224.0 WirePlumber rule checked in file and via pactl, P2P device
    unmanaged, more parser rejections caught, live fstab vs fstab options
  - verify: 7.229.0 drop the WirePlumber soft-mixer checks (rule file and
    pactl)
  - verify: 7.229.0 list stray files beside managed files, package files by
    owner
  - verify: 7.230.0 stray-sibling match drops the ? wildcard; the stray OK row
    prints only after a full sweep
  - verify: 7.231.0 an unreadable sdboot-manage conf.d directory warns instead
    of reporting OK


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


7.137.0 - 7.138.0
-----------------

  - configuration: drop the dormant RY_REMOTE_PLAY_PORTS nftables gate


7.135.0 - 7.136.1
-----------------

  - check: record 60-ry-* drop-ins and masked units absent from MASK


7.130.0 - 7.134.0
-----------------

  - perf: 7.130.0 - 7.131.1 governor and EPP performance, GPU DPM level high


7.123.0 - 7.129.0
-----------------

  - kernel: add mt7925e.disable_aspm=1 and kernel.nmi_watchdog=0
  - dns: pin upstreams in resolved and the NM global-dns section
  - env: FSR4_UPGRADE -> PROTON_FSR4_UPGRADE; drop VKD3D_CONFIG


7.118.0 - 7.122.0
-----------------

  - services: mask ufw instead of removing


7.100.0 - 7.117.0
-----------------

  - verify: 7.100.0 - 7.107.3 SHA256 static checksum, installed vs generated
    bytes


7.99.1 and earlier
------------------

  - initial profile for the Beelink GTR9 Pro
