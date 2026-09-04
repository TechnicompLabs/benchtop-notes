# Storage and I/O

## I/O Scheduler

##### /etc/udev/rules.d/10-iosched.rules
    # BFQ is recommended for slow storage such as rotational block devices and SD cards.
    ACTION=="add|change", SUBSYSTEM=="block", ATTR{queue/rotational}=="1", ATTR{queue/scheduler}="bfq"
    ACTION=="add|change", SUBSYSTEM=="block", KERNEL=="mmcblk?", ATTR{queue/scheduler}="bfq"

    # Kyber is recommended for faster storage such as NVME and SATA SSDs.
    # Note: For very fast NVMe drives, `none` (noop) may outperform kyber since the
    # device's internal scheduler is already optimal and a host-side scheduler adds
    # overhead. Consider `none` for high-end NVMe, `kyber` for SATA SSDs.
    ACTION=="add|change", SUBSYSTEM=="block", ATTR{queue/rotational}=="0", KERNEL=="nvme?n?", ATTR{queue/scheduler}="kyber"
    ACTION=="add|change", SUBSYSTEM=="block", ATTR{queue/rotational}=="0", KERNEL=="sd?", ATTR{queue/scheduler}="kyber"
[Link](https://github.com/pop-os/default-settings/pull/149)

## Access Times

Use `relatime` as the default mount option (this is already the kernel default since Linux 2.6.30). `relatime` only updates the access time (atime) if the previous atime is older than the modify or change time, or if the previous atime is more than 24 hours old. This reduces write overhead vs. the legacy `atime` behavior while still satisfying applications that depend on atime (e.g., tmpwatch, mutt).

For workloads that never need atime (e.g., build servers, databases), `noatime` eliminates the overhead entirely but can break some tools that depend on it. `relatime` is the safe default for a general-purpose desktop distro.

##### /etc/fstab
    # Example: ensure relatime is set (should be default, but be explicit)
    UUID=xxx  /  btrfs  defaults,relatime,compress=zstd  0  0

## Staggered Spin-Up

NOTE: I had trouble with SSS and thus I believe it should be disabled.

"Some hardware implements staggered spin-up, which causes the OS to probe ATA interfaces serially, which can spin up the drives one-by-one and reduce the peak power usage. This slows down the boot speed, and on most consumer hardware provides no benefits at all since the drives will already spin-up immediately when the power is turned on."

To check if SSS is being used:
    dmesg | grep SSS

To disable it, add the `libahci.ignore_sss=1` kernel parameter.
[Arch Wiki](https://wiki.archlinux.org/title/Improving_performance/Boot_process)

## USB Disks

### Synchronous Writes on Slow USB Flash Drives
USB flash drives have extremely slow random write performance and poor wear leveling. When the kernel's writeback cache fills and flushes to a slow USB stick, the system can appear to hang. Options:
* Set shorter dirty writeback timeouts for removable devices via udev rules
* Consider adding `sync` mount option for USB sticks (trades throughput for data safety)
* Use `udisks2` settings to mount removable media with `flush` option (FAT-specific, flushes after each write)

##### /etc/udev/rules.d/11-usb-dirty-writeback.rules
    # Reduce dirty writeback interval for USB storage to 5 seconds
    ACTION=="add|change", SUBSYSTEM=="block", ATTRS{removable}=="1", ATTR{bdi/max_ratio}="1"

### TRIM over USB for External SSDs
Many USB-attached SSDs support TRIM/UNMAP via UAS (USB Attached SCSI). Ensure the device is detected as UAS rather than BOT (Bulk-Only Transport):
    lsusb -t   # Check for "Driver=uas"

Enable periodic TRIM for external SSDs:
    sudo systemctl enable --now fstrim.timer

Note: Some USB-SATA bridges do not pass through TRIM commands. Check with `lsblk --discard` — non-zero values in DISC-GRAN and DISC-MAX columns indicate TRIM support.

## /tmp on tmpfs

For a desktop distro, /tmp on tmpfs (RAM-backed) is generally beneficial: it is fast, automatically cleaned on reboot, and reduces SSD write wear. However, it can consume RAM if applications create large temp files (e.g., video editing, compilation).

Recommendation: Keep /tmp on tmpfs (systemd default) but set a size limit:

##### /etc/fstab
    tmpfs  /tmp  tmpfs  defaults,noatime,size=4G  0  0

If the distro's target workloads involve large temp files (e.g., video encoding), consider disabling tmpfs for /tmp:
    sudo systemctl mask tmp.mount

## Optimal Encryption Sector Size

Modern drives with 4096-byte physical sectors should use a 4096-byte encryption sector size for LUKS to avoid read-modify-write overhead. Fedora 35+ defaults to 4096 for new LUKS volumes on drives that report 4K sectors:

    cryptsetup luksFormat --sector-size 4096 /dev/sdX

Check drive sector size:
    cat /sys/block/sdX/queue/physical_block_size
    cat /sys/block/sdX/queue/logical_block_size

NVMe drives almost universally use 4096-byte or 512-byte logical sectors. When logical=512 but physical=4096, aligning the encryption sector size to 4096 still provides a performance benefit.

Note: This only applies to new LUKS volumes. Existing volumes cannot be migrated without re-encryption.
