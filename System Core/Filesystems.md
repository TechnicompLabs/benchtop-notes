# Filesystems

## Modern Filesystem Hierarchy
/boot
/etc/ - /config/
/home
/media/ - /external/
/runtime/
  /dev/ - /runtime/device/
  /proc/ -/runtime/status/
  /run/ - /runtime/ipc/
  /sys/ - /runtime/interface/
  /tmp - /runtime/temp/ (tmpfs)
/software/
  /usr/ - /software/base/
  /nix/ - /software/local/
/srv/ - /served/
/var/ - /state/


## Base Filesystem (COW with Snapshots)
* BTRFS
* bcachefs
* Stratis

## Removable Drives
exFAT for all cross-platform removable drives

## Arrays
ZFS for all arrays
* https://www.poolsman.com/

## FUSE Filesystem Support
    # Note: Original used dnf (Fedora). OpenSUSE equivalents below.
    # Some packages may have different names or require OBS repos.
    sudo zypper install dmg2img simg2img fuse-exfat exfat-utils squashfuse squashfs-tools fuse-sshfs fuse-dislocker fuse-encfs
    # zfs-fuse: Use OpenZFS native packages from OBS instead (see OpenSUSE Configuration)
    # fuse-afp: May need OBS — consider afpfs-ng
    # fuse9p: Rarely needed outside QEMU; skip unless required

Supported formats:
* NTFS (FUSE)
* EXFAT (FUSE)
* FAT32 (FUSE)
* HFS+ (FUSE)
* APFS (FUSE — https://github.com/linux-apfs)
* Android Adoptable Storage (https://nelenkov.blogspot.com/2015/06/decrypting-android-m-adopted-storage.html)
* Samsung Encrypted SD Card ?
* BitLocker ?
* FileVault ?
* ReFS ?

## Compression Format Support
    # Note: Original used dnf (Fedora). OpenSUSE equivalents:
    sudo zypper install cabextract lha arj lzip unrar pax p7zip p7zip-full sharutils xz
    # xar, xdms, unace: May require OBS Packman repo
    # unzix: Extremely niche (ZIX format) — skip unless needed

## Partitioning Tools
* Cockpit
* YAST
* KDE Partition Manager
