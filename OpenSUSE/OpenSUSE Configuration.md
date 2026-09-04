# OpenSUSE Configuration

## Known Weaknesses
* RAID
* Disk Partitioning
* ZFS
* Cryptography
* Remote Management
* Automatic Disk Unlock

## Custom Kernel Modules (OBS)
* NVIDIA Module
* Binder and ASHMEM Modules (for Waydroid)
* ZFS Module
* 1000hz Tick Rate

## OBS Packages
* system76-scheduler
* adw-gtk3
* pipewire-nonfree-codecs
* dislocker

## Flatpak Packages
* Waydroid:
    * https://gist.github.com/Saren-Arterius/c5bc39199552a5c244449b0ce467d6b6
    * https://bugzilla.opensuse.org/show_bug.cgi?id=1189456
    * https://linux32bituefi.blogspot.com/2021/12/install-waydroid-in-opensuse-tumbleweed.html
    * https://github.com/SGNight/Arm-NativeBridge
* Ventoy
* Cyberchef
* SD Memory Card Formatter for Linux
* RStudio
* Nautilus w/ Sushi
* GNOME Software


---

## From legacy notes: OBS Custom Repository.md
OpenSUSE Kernel:
	NVIDIA Module
	Binder and ASHMEM Modules
	Zfs Module
	1000hz Tick Rate
	system76-scheduler
	adw-gtk3
	pipewire-nonfree-codecs
	dislocker
Flatpak:
	Waydroid:
		https://gist.github.com/Saren-Arterius/c5bc39199552a5c244449b0ce467d6b6
		https://bugzilla.opensuse.org/show_bug.cgi?id=1189456
		https://linux32bituefi.blogspot.com/2021/12/install-waydroid-in-opensuse-tumbleweed.html
	    https://github.com/SGNight/Arm-NativeBridge
	Cyberchef
	SD Memory Card Formatter for Linux
	Nautilus w/ Sushi
	Gnome Software


---

## From legacy notes: OpenSUSE Configuration.md

# Weaknesses
- Disk Partitioning
- Cryptography
- Remote Management
- Automatic Disk Unlock

# Core Components
OpenRGB-udev-rules
rasdaemon

##### Pipewire
 TODO: non-free Codecs:
[Link](https://github.com/mikeroyal/PipeWire-Guide)

## Enable Avahi
    sudo systemctl status avahi-daemon

## Hostname
/etc/nsswitch.conf.d/10-hostname.conf
Change line hosts:  files mdns_minimal [NOTFOUND=return] dns to read:

# Security
OpenSUSE by default disallows wheel sudo; also sets Defaults targetpw.  Need to change both settings and then lock root

### polkit
/polkit-1/rules.d/50-wheel-auth-self.rules

### Performance Monitoring
	Requires:       htop
	Requires:       iotop-c
	Requires:       nvtop
	Requires:       iftop
	Requires:       nethogs
	Requires:       ss
	Requires:       powertop
	Requires:       atop

# ToDo
  * Change to sudo
  * Command Line Interface
