# Power Management

## tuned

tuned is a system tuning daemon that applies pre-defined or custom tuning profiles. It adjusts sysctl settings, I/O scheduler, CPU governor, kernel parameters, and more based on the active profile.

### Generate a custom profile from powertop recommendations
    sudo powertop2tuned --enable custom-desktop
This creates a tuned profile at `/etc/tuned/custom-desktop/tuned.conf` based on powertop's power-saving recommendations. Review and edit the generated profile before deploying — powertop's recommendations are aggressive and may disable USB autosuspend on devices you use frequently.

### Enable tuned
    sudo systemctl enable --now tuned
    tuned-adm active                  # Show current profile
    tuned-adm list                    # Show available profiles
    tuned-adm profile desktop         # Set desktop profile (balanced)
    tuned-adm profile throughput-performance  # For build servers / benchmarks

### Recommended Profiles
* `desktop` — Balanced power/performance for desktop use (inherits from `balanced`, adds input device latency tweaks)
* `latency-performance` — Minimum latency, maximum CPU performance (good for audio production)
* `throughput-performance` — Maximum throughput (good for compilations, servers)
* `powersave` — Maximum battery life
* Custom: Create a desktop-gaming profile that inherits from `desktop` but adds gaming-specific tweaks (THP, scheduler, etc.)

### Custom Profile Example
##### /etc/tuned/desktop-gaming/tuned.conf
    [main]
    include=desktop

    [vm]
    transparent_hugepages=always
    transparent_hugepage.defrag=defer+madvise

    [sysctl]
    kernel.split_lock_mitigate=0
    vm.compaction_proactiveness=0

## Power Profiles Daemon Options
* [system76-power](https://github.com/pop-os/system76-power)
* [tuned](https://tuned-project.org/)
* [TLP](https://linrunner.de/tlp/)
* [power-profiles-daemon](https://gitlab.freedesktop.org/upower/power-profiles-daemon)

## Hardware Monitoring
* [smartmontools](https://www.smartmontools.org/)
* [thermal_daemon](https://github.com/intel/thermal_daemon) — Intel thermal management
* [hdparm](https://wiki.archlinux.org/title/Hdparm)
* [sdparm](https://sg.danny.cz/sg/sdparm.html)

## Power Tweaks Reference
* [Clear Linux Power Tweaks](https://github.com/clearlinux/clr-power-tweaks)

## auto-cpufreq
* [auto-cpufreq](https://github.com/AdnanHodzic/auto-cpufreq)

## Zypper: Automatically Remove Orphaned Packages

By default, zypper does not remove orphaned dependencies when you remove a package. To enable automatic cleanup:

##### /etc/zypp/zypp.conf
    solver.cleandepsOnRemove = true

Alternatively, manually clean orphans:
    sudo zypper packages --orphaned
    sudo zypper remove --clean-deps <package>


---

## From legacy notes: Battery and Power Management.md
powertop2tuned and tuned profile config
Zypper Enable Remove Orphans
