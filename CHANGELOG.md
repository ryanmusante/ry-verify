Changes for ry-verify
=====================

Newest first. Versioning is MAJOR.MINOR.PATCH.

7.214.0
-------

  - report: new --report runs --verify, then writes a self-contained HTML
    report (0600) beside the JSONL log; a failed write turns exit 0 into 1


7.190.0 - 7.212.1
-----------------

  - boot: 7.211.0 the missing-entries hints drop --verbose
  - kernel: 7.195.0 fsck.mode=force; 7.199.0 adds ttm.pages_limit=20971520;
    7.204.0 expects nowatchdog, read back through kernel.watchdog
  - perf: 7.204.0 expects governor powersave with EPP performance; 7.211.0
    expects governor performance again, reverting 7.204.0
  - env: 7.195.0 PROTON_FSR4_INDICATOR=1 dropped; 7.212.1 expects
    RADV_PERFTEST=nggc and MANGOHUD_DLSYM=1
  - configuration: 7.195.0 expects MangoHud to ship cpu_stats enabled;
    7.203.0 the managed-file header on every file but /etc/kernel/cmdline
  - configuration: 7.205.0 cpupower-service.conf header names EnvironmentFile=
  - configuration: 7.211.0 expects udev comments with the set EPP and GPU
    levels and a resolved header without its mDNS/LLMNR claim
  - sysctl: 7.195.0 expects vm.watermark_scale_factor=125
  - verify: 7.195.1 module-parameter expectations derive from KERNEL_PARAMS;
    7.197.0 exact integer compare; 7.198.0 reads back every managed token
  - verify: 7.200.0 asserts connectivity disabled; 7.202.0 fails a symlinked
    destination in the checksum phase, 7.207.0 once, its perms row INFO
  - verify: 7.206.0 bluetooth keys in emission order; 7.207.0 a sudo bail
    closes its VERIFICATION section
  - verify: 7.203.0 - 7.208.0 labels read Wi-Fi and kernel.watchdog; 7.211.0
    the powerdevil crash hint matches coredumps by executable path
  - verify: 7.211.0 active but not-enabled units warn; unset ENV_VARS name
    systemctl --user daemon-reload
  - check: 7.195.1 - 7.195.2 silence every bootstrap gate; 7.200.1 logs
    CHECK_*_DRIFT with the cause; 7.202.0 records CHECK_SYMLINK_DRIFT
  - preflight: 7.211.0 a missing root UUID names the findmnt exit code or an
    empty result
  - cli: 7.211.0 glued short flags take only h and v; -Vh and -hV fail like -V
  - logging: 7.211.0 a caught signal logs before its stderr line
  - split: 7.190.0 moves ry-verify.fish here


7.139.0 - 7.189.0
-----------------

  - boot: COMPRESSION_OPTIONS -1 -> -3, drop -T0; fsck.mode=force -> auto
  - kernel: land on iommu=pt; drop amd_iommu, clearcpuid=umip, amdxdna
  - dns: drop pinned upstreams, DNSOverTLS= and DNSSEC=; link DNS wins
  - network: autoconnect-retries-default=0
  - env: PROTON_FSR4_UPGRADE -> FSR4_WATERMARK -> PROTON_FSR4_INDICATOR=1;
    GSK_RENDERER ngl then gl
  - configuration: assert the nftables ICMPv6 base accept
  - packages: 7.173.0 adds cachyos-benchmarker
  - sysctl: drop both net.core.netdev_budget keys and vm.swappiness=150
  - backup: .ry.bak moves to ~/ry-install/backups; .ry.orig strays are INFO
  - verify: checks every cpufreq policy and non-fallback loader entries,
    resolved unit state, live ext4 opts, MangoHud, .ry.bak, parser complaints
  - check: mode drift sets drift; 7.186.1 - 7.187.0 take every --check
    abbreviation, silent rc 3
  - preflight: rc 3 on a reserved COUNTRY, NM_WIFI_POWERSAVE outside 0-3
  - split: 7.177.0 moves verify and check to ry-verify.fish


7.137.0 - 7.138.0
-----------------

  - configuration: drop the dormant RY_REMOTE_PLAY_PORTS nftables gate


7.135.0 - 7.136.1
-----------------

  - check: records 60-ry-* drop-ins and masked units absent from MASK


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
