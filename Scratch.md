https://github.com/kmscon/kmscon

Gnome Extensions
https://github.com/NarkAgni/dhruva
https://github.com/bjuergens/whatcable-gnome
https://github.com/AnnoyingTechnology/gnome-extension-cool-my-ryzen-ai-max

Applications
https://github.com/wimpysworld/sidra

---

## From legacy notes: Scratch.md

### App Installation
https://github.com/linuxmint/webapp-manager
https://www.appimagehub.com/
https://www.appimagehub.com/p/1228228
https://github.com/prateekmedia/appimagepool

### Fonts
https://news.ycombinator.com/item?id=30705078
https://github.com/pdeljanov/infinality-remix/issues/13
https://github.com/pdeljanov/infinality-remix
https://gist.github.com/cryzed/e002e7057435f02cc7894b9e748c5671
https://wiki.archlinux.org/title/Font_configuration

##### System
https://wiki.archlinux.org/title/Improving_performance
https://wiki.archlinux.org/title/Gaming#Improving_performance
https://wiki.archlinux.org/title/sysctl
https://wiki.archlinux.org/index.php/Hardware_video_acceleration
https://www.clearlinux.org/clear-linux-documentation/guides/index.html
https://www.clearlinux.org/clear-linux-documentation/guides/clear/performance.html

##### Kernel
https://liquorix.net/#features
https://github.com/zen-kernel/zen-kernel/wiki/Detailed-Feature-List
https://github.com/clearlinux-pkgs/linux/
https://xanmod.org/
https://news.ycombinator.com/item?id=46366998

##### Distro Defaults
https://github.com/pop-os/default-settings/
https://github.com/CachyOS/CachyOS-Settings

##### Applications
https://github.com/FeralInteractive/gamemode/
https://github.com/pop-os/system76-scheduler
https://wiki.archlinux.org/title/Preload
https://handwiki.org/wiki/Prelink
https://github.com/Nefelim4ag/Ananicy
https://github.com/hakavlad/nohang
https://github.com/AdnanHodzic/auto-cpufreq
https://github.com/oracle/bpftune
https://github.com/ZorinOS/zorin-exec-guard

##### Distro Defaults
https://github.com/clearlinux/clr-power-tweaks

##### Power Profiles Daemon
https://github.com/pop-os/system76-power
https://tuned-project.org/
https://linrunner.de/tlp/
https://gitlab.freedesktop.org/upower/power-profiles-daemon

##### Applications
https://www.smartmontools.org/
https://github.com/intel/thermal_daemon
https://wiki.archlinux.org/title/Hdparm
https://sg.danny.cz/sg/sdparm.html

### Security
https://kspp.github.io/
https://www.kicksecure.com/wiki/Security-misc
https://github.com/Kicksecure/security-misc
https://www.clearlinux.org/clear-linux-documentation/guides/clear/security.html
https://github.com/evilsocket/opensnitch

##### Cryptography
https://github.com/canonical/crypto-config
https://gitlab.com/redhat-crypto/fedora-crypto-policies
https://en.opensuse.org/SDB:Crypto-policies
Idea: fTPM Integration, YubiKey, NitroKey Integration

### Rust
https://discourse.ubuntu.com/t/carefully-but-purposefully-oxidising-ubuntu/56995
https://www.phoronix.com/news/Ubuntu-25.10-sudo-rs-Default
https://ubuntu.com/blog/tpm-backed-full-disk-encryption-is-coming-to-ubuntu

### Kernel Documentation
https://www.kernel.org/doc/html/latest
https://lwn.net/Kernel/Index/
https://kernelnewbies.org/
https://wiki.archlinux.org/title/Kernel_parameters

### Hardware Specific Tools
https://openrgb.org/
https://openrazer.github.io/
https://github.com/linux-surface
https://asus-linux.org/
https://docs.mrchromebox.tech/
https://asahilinux.org/
https://t2linux.org/

### Accessibility
http://fireborn.mataroa.blog/blog/i-want-to-love-linux-it-doesnt-love-me-back-post-1-built-for-control-but-not-for-people/

##### Tailscale
https://news.ycombinator.com/item?id=46531925
https://github.com/tailscale/tailscale/issues/17654
https://github.com/tailscale/tailscale/issues/18288
https://github.com/tailscale/tailscale/issues/18302

##### Aeon
https://www.reddit.com/r/AeonDesktop/comments/1pwuvnw/pcr15_validation_again_unable_to_reenroll_please/
https://www.reddit.com/r/AeonDesktop/comments/1o9wip0/the_validation_of_pcr_15_failed/

### Partitioning Apps
KDE Partition Manager

### Supported Models
Laptops:
  Apple MacBook
  Microsoft Surfcace
  ASUS ROG/ProARt
  Razer Blade
  Lenovo ThinkPad
  Dell Precision/Latitude/XPS
  HP Elitebook
  HP Zbook
  
### CachyOS Optimizations
CachyOS Kernel
    Uses the BORE scheduler.
    Built with clang and ThinLTO
    Profiled with our own AutoFDO Profile
        Script used to profile the kernel.
   •	Choose between 3 kernel schedulers and various sched-ext schedulers for improved responsiveness
	•	AMD P-State Improvements
	•	Latest BBRv3 by Google
	•	le9uo for significantly improved responsiveness during high memory load
	•	Up-to-date NTSYNC patchset, used with a compatible build of wine/proton
	•	Compatibility with T2 MacOS devices with patches from t2linux
	•	Allows reading per-core CPU energy usage for AMD users
	•	ACS Override and v412loopback
	•	VHBA module for emulating CD/DVD-ROM devices
	•	Latest ZSTD patchset
	•	Various other patches that focus on improving performance (optimized compiler flags, cryptographic improvements, memory management tweaks)
	•	x86-64-v3: 5%-20% performance uplift compared to x86-64.
	•	x86-64-v4: Delivers substantial performance gains through AVX512 support, depending on the workload.
	•	Zen 4/5: In addition to the x86-64-v4 instruction set, the following are added:

##### Transparent Hugepages
I propose we enable THP and disable proactive compaction for gaming sessions and set these back to default when the session ends (as some workloads such as databases can be negatively impacted by memory fragmentation that this can cause).
My proposal is to enable the following when gaming and restore when game ends:
See e.g. [https://github.com/CryoByte33/steam-deck-utilities/blob/main/docs/tweak-explanation.md](https://github.com/CryoByte33/steam-deck-utilities/blob/main/docs/tweak-explanation.md)
See also [https://blog.patshead.com/2023/02/enabling-transparent-hugepages-can-provide-huge-gaming-performance-improvements.html](https://blog.patshead.com/2023/02/enabling-transparent-hugepages-can-provide-huge-gaming-performance-improvements.html)
See also [https://alexandrnikitin.github.io/blog/transparent-hugepages-measuring-the-performance-impact/](https://alexandrnikitin.github.io/blog/transparent-hugepages-measuring-the-performance-impact/)

##### Tick Rate
CONFIG_HZ=1000 last but not least, the only option that is *only* tunable at compile time. As already mentioned there is a potential risk of regressions for CPU-intensive applications, but they can be mitigated (and maybe they could even outperformed) with NO_HZ_FULL. On the other hand, HZ=1000 can improve system responsiveness, that means most of the desktop and server applications will benefit from this (the largest part of the server workloads is I/O bound, more than CPU-bound, so they can benefit from having a kernel that can react faster at switching tasks), not to mention the benefit for the typical end users applications (gaming, live conferencing, multimedia, etc.).
USB Disks
  1.  Synchronous writes on slow disks (USB Flash drives)
  2. TRIM over USB for fast disks
GNOME Settings
    https://github.com/pop-os/firmware-manager
    https://www.reddit.com/r/gnome/comments/qyi9lc/does_gnome_plans_to_integrate_firewall_settings/
Disable tmp on tmpfs
ZRAM Hibernate
  Linux Resume:
    Reset Bluetooth
    Reset WiFi
    Reset Trackpad
    Reset Touchscreen
CryptSetup Opal
Facebook BOLT
Optimal Encryption Sector Size (Fedora 35)
Filesystems:
Linux patching research
  Theory: rolling release patches security bugs faster than forked LTS
  Theory: pip/npm better than distro-managed libraries
https://github.com/vinceliuice/MacTahoe-gtk-theme
https://news.ycombinator.com/item?id=46031208
https://github.com/somepaulo/MoreWaita
Enable 2MB THP by Default
https://www.phoronix.com/news/Glibc-malloc-2MB-THP-AArch64
Libreoffice writer did not respect dark mode change and icons do not show up
Browser Cache
**Realtime**
Group Limits
User pmartin is currently not member of a group that has sufficient rtprio (0) and memlock (-1) set. Add yourself to a group with sufficent limits set, i.e. audio or realtime, with 'sudo usermod -a -G <group_name> pmartin. See also https://wiki.linuxaudio.org/wiki/system_configuration#audio_group
RT Priorities
Could not assign a 80 rtprio SCHED_FIFO value due to the following error: [Errno 1] Operation not permitted. Set up limits.conf. See also https://wiki.linuxaudio.org/wiki/system_configuration#limitsconfaudioconf
Power Management
Power management can't be controlled from user space, the device node /dev/cpu_dma_latency can't be accessed by your user. This prohibits DAWs like Ardour and Reaper to set CPU DMA latency which could help prevent xruns. For enabling access see https://wiki.linuxaudio.org/wiki/system_configuration#quality_of_service_interface
Swappiness
vm.swappiness is set to 180 which is too high. Set swappiness to a lower value by adding 'vm.swappiness=10' to /etc/sysctl.conf and run 'sysctl --system'. See also https://wiki.linuxaudio.org/wiki/system_configuration#sysctlconf

## Modern Linux:
	Everything is really a file
	Text format for all personal data; greppable
	Tag-based FS Tags Cross Platform into Virtual Folders
	    Implement Tags using hard link so a vnode with two links is given two “tags” in Dolphin
	    Look into ZFS Hard Link and Tag Support
	ZFS Root Distributed with Low Latency Kernel
	Subvolume for /home or /home on Separate Array
	No Support for distributed /
	Fix Linux Kernel in ESP
	Toggle for “Cross-platform” or “Next Generation” support for /home
		Cross-platform
			No hard links
			Case insensitive FS
			Limited filename character set
			Full POSIX
		Single Platform
			Tags implemented through hard links
			Case sensitive FS
			Full filename character set
			Ignore POSIX
	Do we need all file metadata attributes?  Can we simply record last file modification time (or creation time if not modified?)
