# GNOME Configuration

## Global Behavior

### Out of Memory User Dialog
https://gitlab.gnome.org/Teams/Design/whiteboards/-/issues/340

### Notifications
Apps should dismiss outdated notifications.

### App Grid
* Alphabetical App Grid (AlphabeticalAppGrid@stuarthayhurst)
* App Hider
* Applications Overview Tooltip (applications-overview-tooltip@RaphaelRochet)
* Vertical App Grid (vertical-app-grid)

### Search
* ESP (Extension Search Provider)
* Gnome Fuzzy App Search (possibly integrated into Shell in future: https://gitlab.gnome.org/GNOME/glib/-/issues/1152)
* WSP (Window Search Provider)

### Drag and Drop Issues
* Drag files to dock
* Drag window while changing desktops
* Drag icons to sidebar
* Drag text/images to desktop
* Cannot drag icon from background window without raising the window
    * https://gitlab.gnome.org/Teams/Design/whiteboards/-/issues/255

### Cloud Sync
* https://gitlab.gnome.org/Teams/Design/whiteboards/-/issues/335
* https://discourse.gnome.org/t/proposal-integrate-syncthing-into-gnome-settings/15387
* Dotfiles, Flatpak Apps, GNOME Shell Extensions, GNOME Shell Configuration

### Fingerprint Reader
https://github.com/xapp-project/fingwit

### Other
* Color Picker: Standalone App + QT + GTK
* Global option to open all files in tabs in QT + GTK applications
* Default templates in Template folder

## Bugs

### GDM
* GDM login screen on wrong display
    * https://gitlab.gnome.org/GNOME/gnome-shell/-/issues/3867
    * https://github.com/thiggy01/change-gdm-background/issues/15

### Endeavor (Tasks App)
* Nested Tasks: https://gitlab.gnome.org/World/Endeavour/-/issues/488

### GNOME Initial Setup
* Wrong WiFi password cannot be re-entered

### Nautilus
* When moving a directory containing a file the user lacks permissions for, half the directory gets moved before the error, leading to an inconsistent state. Moves should verify 100% success before any files are deleted.
* When copying or moving files, copy stops on first error. Copy should continue while error dialog is displayed.

### Shell
* When an application inhibits sleep or reboot there should be a notification.

## Settings Configuration

### User-Configurable (needs GNOME GUI)
* Hostname
* Sync(thing)
* SMB
* Firewall

### System Settings (user shouldn't change)
* Bootloader
* NetworkManager
* Sysctl
* SystemD

## Default Settings

    gsettings set org.gnome.mutter check-alive-timeout 60000
[Link](https://askubuntu.com/questions/412917/how-to-increase-waiting-time-for-non-responding-programs)

    gsettings set org.gnome.nautilus.preferences open-folder-on-dnd-hover true
[Link](https://www.omgubuntu.co.uk/2023/02/ubuntu-open-folder-on-drag-drop-hover)

## GNOME Settings App Additions
* https://github.com/pop-os/firmware-manager
* https://www.reddit.com/r/gnome/comments/qyi9lc/does_gnome_plans_to_integrate_firewall_settings/

## Long Term Improvements

### Text Selection Right Click Options
* Define
* Speak
* Translate

## Icons in Menus
https://blog.jim-nielsen.com/2025/icons-in-menus/

## LibreOffice Issues
* LibreOffice Writer did not respect dark mode change and icons do not show up


---

## From legacy notes: GNOME Configuration.md

## Out of Memory User Dialog
[https://gitlab.gnome.org/Teams/Design/whiteboards/-/issues/340](https://gitlab.gnome.org/Teams/Design/whiteboards/-/issues/340)

## Notifications
Apps Should Dismiss Outdated Notifications

## Search
* Gnome Fuzzy App Search (Possible Integrated into the Shell in the Future: https://gitlab.gnome.org/GNOME/glib/-/issues/1152)

## Drag and Drop
Drag Files to dock
Drag window while changing desktops
Drag icons to sidebar
Drag text/images to desktop
Cannot Drag Icon from background window without raising the window as a result
	https://gitlab.gnome.org/Teams/Design/whiteboards/-/issues/255

#### Cloud Sync
https://gitlab.gnome.org/Teams/Design/whiteboards/-/issues/335
https://discourse.gnome.org/t/proposal-integrate-syncthing-into-gnome-settings/15387
Dotfiles
Flatpak Apps
Gnome Shell Extensions
Gnome Shell Configuration

#### Other
Color Picker: Standalone App + QT + GTK
Global Option to open all files in tabs in QT + GTK Applications
Default Templates in Template Folder

##  GDM
- GDM login screen on wrong display
	- https://gitlab.gnome.org/GNOME/gnome-shell/-/issues/3867
	- https://github.com/thiggy01/change-gdm-background/issues/15

## Endeavor
- Nested Tasks
	- https://gitlab.gnome.org/World/Endeavour/-/issues/488

## GNOME Initial Setup
- GNOME Initial Setup - Wrong WiFi password cannot be re-entered

## Nautilus
When moving a directory containing a file that the user does not have permissions to access, half of the directory gets moved before the error, leading to an inconsistent state.  Instead, moves should first succeed 100% before any files are deleted
When copying or moving files, copy stops on first error.  Instead, copy should continue while error dialog is displayed.

## Shell
When an application inhibits sleep or reboot there should be a notification

## Theming
* Adwaita
    https://github.com/eylles/adw-gtk2-colorizer
    https://github.com/lassekongo83/adw-gtk3
    https://github.com/dp0sk/adw-gimp3
	https://github.com/RichardSepsi/adw-inkscape
	https://github.com/nukusaba/Libadwaita-KDE/
	https://github.com/GabePoel/KvLibadwaita
	https://github.com/rafaelmardojai/firefox-gnome-theme
	https://github.com/rafaelmardojai/thunderbird-gnome-theme
	https://github.com/tkashkin/Adwaita-for-Steam
	https://github.com/piousdeer/vscode-adwaita
	https://github.com/ricewind012/discord-gnome-theme
	https://github.com/birneee/obsidian-adwaita-theme
* Yaru (https://github.com/ubuntu/yaru)
	Solves: Can't Distinguish active from Inactive Window
	Black Headerbar on Active Window; White on Inactive

# Gnome Settings
Configuration Settings (User May Change, needs gnome)
- Hostname
- Sync(thing)
- Firewall
System Settings: (User shouldn’t change)
- Bootloader
- NetworkManager
- SystemD

## Blur My Shell
  https://github.com/aunetx/blur-my-shell/issues/455
  https://github.com/aunetx/blur-my-shell/issues/757

## Coverflow Alt-tab
  https://github.com/dsheeler/CoverflowAltTab/issues/13

## Dash to Dock
Gnome Dock Launch Feedback
  https://github.com/micheleg/dash-to-dock/issues/49
    - https://github.com/home-sweet-gnome/dash-to-panel/issues/2218
  https://github.com/micheleg/dash-to-dock/pull/574
  https://github.com/micheleg/dash-to-dock/issues/1029

## GTK4 Desktop Icons Next Generation (DING)
Include but toggle

## Just Perfection
  Click to Close Overview (Repalce click-to-close-overview@l3nn4rt.github.io)
  Restore Desktop Thumbnails (Replace GNOME 4X UI Improvements)

## Rounded Window Corners Reborn
Unround the bottom corners (Not currently possible)
Corner radius of 15 for consistency between GTK3 and libadwaita
https://github.com/flexagoon/rounded-window-corners/issues/36
https://gitlab.gnome.org/GNOME/gnome-shell/-/issues/7903
https://github.com/flexagoon/rounded-window-corners/issues/43

## Search Light
- Consider
- Integrate with Blur My Shell

## Tiling Shell
- Switch desktop while dragging and hovering on screen edge
- Switch desktops with keyboard while dragging

## Transparent Window Moving
- Switch desktop while dragging and hovering on screen edge
- Switch desktops with keyboard while dragging

## Weather o'Clock
Consider:
Caffeine (caffeine@patapon.info)
Clipboard Indicator (clipboard-indicator@tudmotu.com)
AlphabeticalAppGrid@stuarthayhurst
Always-Show-Titles-In-Overview
applications-overview-tooltip@RaphaelRochet
* Gnome Fuzzy App Search (Possible Integrated into the Shell in the Future: https://gitlab.gnome.org/GNOME/glib/-/issues/1152)
https://extensions.gnome.org/extension/8226/maximize-to-empty-workspace-2025/
Wifi QR Code
Bangs Search
