Changes for ry-verify
=====================

Newest first. Versioning is MAJOR.MINOR.PATCH.

7.217.0
-------

  - kernel: drop ttm.pages_limit=20971520
  - env: RADV_PERFTEST=nggc -> nggc,nircache


7.190.0 - 7.216.0
-----------------

  - boot: 7.211.0 the missing-entries hints drop --verbose
  - kernel: 7.195.0 fsck.mode=force
  - kernel: 7.199.0 add ttm.pages_limit=20971520
  - kernel: 7.204.0 expect nowatchdog, read back through kernel.watchdog
  - perf: 7.204.0 expect governor powersave with EPP performance
  - perf: 7.211.0 expect governor performance again
  - env: 7.195.0 drop PROTON_FSR4_INDICATOR=1
  - env: 7.212.1 expect RADV_PERFTEST=nggc and MANGOHUD_DLSYM=1
  - configuration: 7.195.0 expect MangoHud cpu_stats enabled
  - configuration: 7.203.0 expect the managed-file header on every file but
    /etc/kernel/cmdline
  - configuration: 7.205.0 cpupower-service.conf header names EnvironmentFile=
  - configuration: 7.211.0 expect udev comments with the set EPP and GPU
    levels
  - configuration: 7.211.0 expect a resolved header without its mDNS/LLMNR
    claim
  - sysctl: 7.195.0 expect vm.watermark_scale_factor=125
  - verify: 7.195.1 module-parameter expectations derive from KERNEL_PARAMS
  - verify: 7.197.0 exact integer compare
  - verify: 7.198.0 read back every managed token
  - verify: 7.200.0 assert connectivity disabled
  - verify: 7.202.0 fail a symlinked destination in the checksum phase
  - verify: 7.207.0 fail a symlinked destination once, its perms row INFO
  - verify: 7.206.0 bluetooth keys in emission order
  - verify: 7.207.0 a sudo bail closes its VERIFICATION section
  - verify: 7.203.0 - 7.208.0 labels read Wi-Fi and kernel.watchdog
  - verify: 7.211.0 the powerdevil crash hint matches coredumps by executable
    path
  - verify: 7.211.0 active but not-enabled units warn
  - verify: 7.211.0 unset ENV_VARS name systemctl --user daemon-reload
  - check: 7.195.1 - 7.195.2 silence every bootstrap gate
  - check: 7.200.1 log CHECK_*_DRIFT with the cause
  - check: 7.202.0 record CHECK_SYMLINK_DRIFT
  - report: 7.214.0 --report runs --verify, then writes a self-contained HTML
    report (0600) beside the JSONL log
  - report: 7.214.0 a failed report write turns exit 0 into 1
  - report: 7.215.0 profile states grade as the ledger does
  - report: 7.215.0 not installed, running but not enabled, and still
    installed warn; the rest fail
  - report: 7.215.0 a masked unit still active, a deployed parameter not yet
    live, an unreadable managed file, and a sudo lapse fail
  - report: 7.215.0 a --report run logs to report-*.jsonl beside its HTML
  - report: 7.215.0 a zero maximum clock no longer divides by zero
  - report: 7.215.0 GPU IDs stay aligned when a sysfs node is unreadable
  - report: 7.215.0 a plain /boot directory no longer repeats the / storage
    meter
  - preflight: 7.211.0 a missing root UUID names the findmnt exit code or an
    empty result
  - cli: 7.211.0 glued short flags take only h and v; -Vh and -hV fail like -V
  - cli: 7.215.0 --help names the report-not-written meaning of exit 1
  - logging: 7.211.0 a caught signal logs before its stderr line
  - split: 7.190.0 move ry-verify.fish here


7.139.0 - 7.189.0
-----------------

  - boot: COMPRESSION_OPTIONS -1 -> -3, drop -T0
  - boot: fsck.mode=force -> auto
  - kernel: land on iommu=pt
  - kernel: drop amd_iommu, clearcpuid=umip, amdxdna
  - dns: drop pinned upstreams, DNSOverTLS= and DNSSEC=; link DNS wins
  - network: autoconnect-retries-default=0
  - env: PROTON_FSR4_UPGRADE -> FSR4_WATERMARK -> PROTON_FSR4_INDICATOR=1
  - env: GSK_RENDERER ngl then gl
  - configuration: assert the nftables ICMPv6 base accept
  - packages: 7.173.0 add cachyos-benchmarker
  - sysctl: drop both net.core.netdev_budget keys
  - sysctl: drop vm.swappiness=150
  - backup: .ry.bak moves to ~/ry-install/backups
  - backup: .ry.orig strays are INFO
  - verify: check every cpufreq policy and non-fallback loader entries
  - verify: check resolved unit state, live ext4 opts, MangoHud, .ry.bak and
    parser complaints
  - check: mode drift sets drift
  - check: 7.186.1 - 7.187.0 take every --check abbreviation, silent rc 3
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

  - dns: pin upstreams in resolved and the NM global-dns section
  - kernel: add mt7925e.disable_aspm=1 and kernel.nmi_watchdog=0
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
