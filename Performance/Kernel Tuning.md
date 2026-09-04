# Kernel Tuning

## Preemption

##### /etc/kernel/cmdline
    +"preempt=full"

Note: full vs. voluntary preemption can cause a significant degradation in throughput.

###### 2024-09-07 Note (RESOLVED): PREEMPT_RT merged into mainline Linux 6.12 (Nov 2024)
PREEMPT_RT is now available in mainline. For kernels 6.12+, consider using `preempt=full` with the RT patches enabled rather than just `preempt=full` on a non-RT kernel. This should be the preferred configuration for a desktop distro.

Context from the original discussion:
"It should be the default in distributions. Exceptions might be worth evaluating for server-specific kernels, but even then I suspect they would find that the throughput penalty is not large enough to make it worth disabling.

Without PREEMPT_RT, nevermind forcibly large audio buffers, Linux hiccups are so severe they can even be visible as on-screen stutter.

PREEMPT_RT makes Linux better than both Windows and MacOS for realtime."
[Link](https://www.phoronix.com/forums/forum/phoronix/latest-phoronix-articles/1490123-linux-very-close-to-enabling-real-time-preempt_rt-support/page2)
[Link](https://lwn.net/Articles/994322/)

## Interrupts

##### /etc/kernel/cmdline
    +="threadirqs"
    +="rcu_nocbs=all"
    +="rcutree.enable_rcu_lazy=1"
     sdbootutil update-all-entries
[Link](https://lwn.net/Articles/931920/)

##### IRQ Balancing
	systemctl enable --now irqbalance.service

## Watchdog

##### /etc/kernel/cmdline
    +="nowatchdog"
    +="nmi_watchdog=0"
      sdbootutil update-all-entries
##### /etc/modprobe.d/disable-sp5100-watchdog.conf
    blacklist sp5100_tco
##### /etc/modprobe.d/disable-TCO-watchdog.conf
    blacklist iTCO_wdt

## Realtime

	sudo systemctl enable --now rtkit-daemon.service

rtkit-daemon (RealtimeKit) is a D-Bus system service that allows user processes to acquire realtime scheduling priority without being root. It does this safely by limiting the maximum realtime priority and watchdogging processes to prevent system lockups. Required for PipeWire and other audio/media applications to achieve low-latency scheduling.

## Tick Rate

CONFIG_HZ=1000 — the only option that is *only* tunable at compile time. There is a potential risk of regressions for CPU-intensive applications, but they can be mitigated (and maybe even outperformed) with NO_HZ_FULL. On the other hand, HZ=1000 can improve system responsiveness — most desktop and server applications benefit from this (the largest part of server workloads is I/O bound, more than CPU-bound, so they benefit from a kernel that can react faster at switching tasks), not to mention the benefit for typical end user applications (gaming, live conferencing, multimedia, etc.).

## Split Lock Mitigate

In some cases, split lock mitigate can slow down performance in some applications and games.

##### /etc/sysctl.d/99-splitlock.conf
    kernel.split_lock_mitigate = 0
[Link](https://www.phoronix.com/news/Linux-Splitlock-Hurts-Gaming)
[Link](https://github.com/doitsujin/dxvk/issues/2938)

## CachyOS Kernel Reference

CachyOS Kernel:
* Uses the BORE scheduler
* Built with clang and ThinLTO
* Profiled with AutoFDO
* Choose between 3 kernel schedulers and various sched-ext schedulers for improved responsiveness
* AMD P-State Improvements
* Latest BBRv3 by Google
* le9uo for significantly improved responsiveness during high memory load
* Up-to-date NTSYNC patchset (used with compatible wine/proton builds)
* Compatibility with T2 MacOS devices via t2linux patches
* Per-core CPU energy usage reading for AMD
* ACS Override and v4l2loopback (virtual camera device for OBS, etc.)
* VHBA module for emulating CD/DVD-ROM devices
* Latest ZSTD patchset
* Various other patches (optimized compiler flags, cryptographic improvements, memory management tweaks)

Architecture targets:
* x86-64-v3: 5%-20% performance uplift compared to x86-64
* x86-64-v4: Substantial performance gains through AVX512 support (workload-dependent)
* Zen 4/5: x86-64-v4 instruction set plus additional extensions


---

## From legacy notes: Linux Desktop Performance.md

##### /etc/kernel/cmdline
Note: full vs. voluntary preemption can cause a significant degradation in throughput

###### 2024-09-07 Note: preempt-rt will merge soon and should possibly be used instead
"It should be the default in distributions. Exceptions might be worth evaluating for server-specific kernels, but even then I suspect they would find that the throughput penalty is not large enough to make it worth disabling
IMHO the timing couldn't be better (the jokes make themselves) with Linux now dealing with a wave of new desktop users. Windows isn't the best at realtime, but is much better than non-rt Linux, whereas PREEMPT_RT makes Linux better than both Windows and MacOS."

    # Higher values encourage the kernel to be more eager to move pages to swap.
  This is already the default for systemd-zram-service
TODO: Switch from ZRAM to ZSWAP and Support Hibernate

	# Format: type path mode user group age argument
Note: there is a lot of debate about these settings.  Depending on the workload it can help performance, hurt performance (on memory pressure) or make no difference.  Always is opt-out and madvise is opt-in.   For gaming you want always but for desktop responsiveness you want opt-in because page merging actually results in jitter.
[Link](https://www.kernel.org/doc/html/latest/admin-guide/mm/transhuge.html) [Link](https://www.reddit.com/r/archlinux/comments/1atueo0/higher_ram_usage_since_kernel_67_and_the_solution/) [Link](https://github.com/CryoByte33/steam-deck-utilities/blob/main/docs/tweak-explanation.md) [Link](https://blog.nelhage.com/post/transparent-hugepages/) [Link](https://www.evanjones.ca/hugepages-are-a-good-idea.html)  [Link](https://stackoverflow.com/questions/11543748/why-is-the-page-size-of-linux-x86-4-kb-how-is-that-calculated/50033983#50033983)

    # Kyber is recommended for faster storage such as NVME and SATA SSDs.
  Link[https://github.com/pop-os/default-settings/pull/149]

#### Staggered Spin-Up
NOTE: I think I had trouble with SSS and thus I believe it should be disabled.
"Some hardware implements [staggered spin-up](https://en.wikipedia.org/wiki/Spin-up#Staggered_spin-up "wikipedia:Spin-up"), which causes the OS to probe ATA interfaces serially, which can spin up the drives one-by-one and reduce the peak power usage. This slows down the boot speed, and on most consumer hardware provides no benefits at all since the drives will already spin-up immediately when the power is turned on. To check if SSS is being used:
 `dmesg | grep SSS`
If it was not used during boot, there will be no output.
To disable it, add the `libahci.ignore_sss=1` [kernel parameter](https://wiki.archlinux.org/title/Kernel_parameter "Kernel parameter")."

# Boot
    mkdir -p /etc/systemd/systemd.conf.d

# Udev
https://github.com/ublue-os/packages/tree/main/packages/ublue-os-udev-rules/src/udev-rules.d Disabled
