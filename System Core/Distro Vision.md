# Distro Vision

## Core Principles
* Immutable base OS with simplified core
* Rolling release for fast security patches
* Drop legacy dependencies (GTK2, Python2)
* Userspace-only support for legacy filesystems and file sharing protocols
* Complete driver and firmware packaging built-in (GPU, WiFi, printers, scanners, function keys)
* Proprietary drivers and firmware included

## Subsystems
* Windows (compatibility layer)
* Android (Waydroid)

## Package Management
* Nix for CLI packages
* FlatPak for GUI packages

## Settings Sync
* Home Manager (Nix) or chezmoi (git) for dotfiles sync
* Fork of Extension Sync for GNOME state

## Desktop Administration
* Linux OOM/Low Mem GUI
* Linux Firewall GUI (Advanced/Configurable)
* File Sharing GUI

## Distro Configured Use Cases
* Machine Learning (CUDA, ROCm)
* Data Science
* Virtualization (KVM, libvirt; GUI: Virt-Manager, Boxes; Web: Incus)
* Software Development (Git, Podman, Kubernetes; GUI: Podman Desktop)
* System Administration (Cockpit, Distrobox/Distrosheff)
* Gaming
* Audio and Multimedia

## Quality of Life
* Starship terminal prompt enabled by default
* Solaar — included for managing Logitech mice along with libratbagd
* Extra udev rules for game controllers and other devices out of the box

## Inspiration
"Bluefin specifically ships upstream tools in lieu of custom applications. The idea of a 'distribution app store' has proven to be unsustainable for desktop application authors, so Bluefin ships tools like Bazaar and Homebrew instead. Additionally the team purposely ensures that the workflows used in Bluefin remain not only distribution agnostic, but operating system agnostic."

* https://docs.projectbluefin.io/introduction
* https://docs.projectbluefin.io/bluefin-dx
* https://docs.projectbluefin.io/command-line

## Modern Linux Filesystem Philosophy

* Everything is really a file
* Text format for all personal data; greppable
* Tag-based FS Tags cross-platform into virtual folders
    * Implement tags using hard links so a vnode with two links is given two "tags" in Dolphin
    * Look into ZFS hard link and tag support
* ZFS root distributed with low latency kernel
* Subvolume for /home or /home on separate array
* No support for distributed /
* Fix Linux Kernel in ESP
* Toggle for "Cross-platform" or "Next Generation" support for /home:
    * Cross-platform: No hard links, case insensitive FS, limited filename character set, full POSIX
    * Single Platform: Tags via hard links, case sensitive FS, full filename character set, ignore POSIX
* Do we need all file metadata attributes? Can we simply record last file modification time (or creation time if not modified)?

## Linux Patching Research
* Theory: rolling release patches security bugs faster than forked LTS
* Theory: pip/npm better than distro-managed libraries


---

## From legacy notes: Immutable Distro Vision.md

# Subsystems
* Windows

# Security
  * Rolling Release Patches
  * Userspace-only Support for

# Filesystems:
Base Filesystem COW w/ Snapshots
	bcachefs

# Pre-Packaged Environment
Configure Compression and Filesystem Format Support
    1.  Compression
        sudo dnf install cabextract lha arj lzip unrar pax p7zip p7zip-plugins p7zip-doc sharutils unzix unace xar xdms xz
    2.  Filesystems
        sudo dnf install dmg2img simg2img fuse-exfat exfat-utils squashfuse squashfs-tools zfs-fuse fuse-afp fuse9p squashfuse orangefs-fuse fuse-sshfs fuse-dislocker fuse-encfs
          NTFS (FUSE)
          EXFAT (FUSE
          FAT32 (FUSE)
          HFS+ (FUSE)
          APFS (FUSE https://github.com/linux-apfs)
          Android Adoptable Storage (https://nelenkov.blogspot.com/2015/06/decrypting-android-m-adopted-storage.html)
          Samsung Encrypted SD Card ?
          BitLocker ?
          FileVault ?
    3.  yq: command-line YAML, JSON, XML, CSV and properties processor
          https://github.com/mikefarah/yq
    4.  pandoc: command-line document conversion format
    5.  Video Codecs
    6.  Image Formats

# Fonts
Infinality Font Rendering
ClearType Fonts
Microsoft TTF Support (and default for OpenOffice)

# Modernized UNIX Protocols?
  Bonjour Service Dsicovery: Avahi
  Syncthing (In-Network Data Exchange)
  Cryptomator Cloud Encryption
  LUKS Volume Encryption
  Nextcloud
  Localsend
  SMB3 File Sharing (Considering whether to modify for lowest common denominator or highest)
	*	Driverless AirPrint/IPP/WSD Printing/Scanning (CUPS/SANE)
	*	RDP low latency replacement - moonlight?
	*	ZFS: https://www.poolsman.com/

# Audio Cues
  Any delayed response/action
  Drag and drop/file copy
  File download
  Empty trash
  Action not allowed
    E.g. click outside box when input required
  https://utcc.utoronto.ca/~cks/space/blog/linux/SystemSoundsShouldBeGranular

# Drivers
Complete Driver Packaging and Firmware Built-In
	Printers, Scanners, GPU, WiFi, Function Keys
	Proprietary Drivers built-in
	Proprietary Firmware built-in
	Optimus/Dynamic GPU Support


---

## From legacy notes: Linux Distro Ideas.md
Distro Configured Uses:
	Machine Learning
	Data Science
	Virtualization
	  GUI: Virt-Manager; Boxes
	  Web (Incus)
	Software Development
	  Kubernetes
	  GUI: Podman Desktop
	System Administration
	  Distrobox/Distrosheff
	Audio and Multimedia
	  Flatpak (Consider)
	    Bazaar Application Store
	    Warehouse for Flatpak management
	    Flatseal
		Consider as Per-user application environment; instead of distrobox
		    “Bluefin specifically ships upstream tools in lieue of custom applications. The idea of a "distribution app store" has proven to be unsustainable for desktop application authors, so Bluefin ships tools like Bazaar and Homebrew instead. Additionally the team purposely ensures that the workflows used in Bluefin remain not only distribution agnostic, but operating system agnostic. For example, podman, docker, and flatpak instead of distribution specific tooling, etc.”
Tailscale Integrate with NetworkManager
rclone - mount nearly any remote storage service onto your local machine, great for multi-machine setups
restic - A modern backup program for your files
** Quality of Life Features **
Starship terminal prompt enabled by default
Solaar - included for managing Logitech mice along with libratbagd
Extra udev rules for game controllers and other devices included out of the box
https://docs.projectbluefin.io/introduction
https://docs.projectbluefin.io/bluefin-dx
https://docs.projectbluefin.io/command-line
Modern Filesystem Hierarchy
