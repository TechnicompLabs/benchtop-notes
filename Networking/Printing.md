# Printing (IPP Driverless Only)

## Architecture

AirPrint is simply IPP Everywhere with Apple-defined capability profiles:
* Transport: IPP over HTTP
* Formats: PDF or Apple Raster
* Discovery: mDNS / Bonjour
Supporting IPP Everywhere automatically supports AirPrint.

Mopria is an industry consortium that standardized driverless IPP printing across vendors:
* IPP Everywhere + Mopria job-ticket profiles
* PDF / PCLm / JPEG raster
* mDNS or WSD discovery

Every major enterprise printer built in the last decade is Mopria-certified.

Microsoft has moved to the same model:
1. V4 Print Driver Model — Modern Windows avoids vendor PCL/PS drivers and relies on class drivers
2. Microsoft IPP Class Driver — Default for all network and USB network-function printers. Conforms to: IPP Everywhere, Mopria, WSD/WS-Print (for discovery only; job transport increasingly IPP)
3. Windows supports IPP over USB, the same profile used on ChromeOS, iOS, and Linux

## What This Distro Needs

Provide:
* CUPS with full IPP Everywhere support
* mDNS/Bonjour discovery (Avahi)
* PDF and PWG/Apple Raster filters
* IPP-over-USB (ipp-usb)

This supports:
* AirPrint
* Mopria
* Windows Modern Print (IPP Class Driver)
* ChromeOS
* Android
* All current enterprise MFPs that implement IPP Everywhere

No proprietary drivers required unless a user insists on vendor-specific finishing or accounting extensions.

## Packages to Install

### Core Printing (CUPS with IPP Everywhere)
* cups
* cups-filters
* cups-filters-ipp
* cups-filters-ghostscript
You do not need Gutenprint or PPD collections if you are strictly driverless.

### Discovery (AirPrint / Mopria)
* avahi
* avahi-utils
Avahi provides Bonjour/mDNS service advertisements and discovery.

### IPP over USB
* ipp-usb
Replaces usbbackend with a proper IPP service over a localhost port. All modern USB printers use this.

## Packages NOT Needed
* No HPLIP
* No Epson ESC/P-R
* No Canon UFR2
* No Brother LPR
* No foomatic-db
* No PPD packs
* No proprietary filters

## Enable CUPS
    sudo systemctl enable --now cups

## Driverless Scanning

Modern MFPs (multifunction printers) support driverless scanning via two protocols:

### eSCL (AirScan)
eSCL is Apple's scanning protocol (part of AirPrint). Most modern MFPs support it. On Linux, use the `sane-airscan` backend:

    sudo zypper install sane-airscan

This provides a SANE backend (`airscan`) that auto-discovers eSCL-capable scanners via mDNS. No configuration needed — it works out of the box with `simple-scan`, GNOME Document Scanner, or any SANE-compatible application.

### WSD (Web Services for Devices)
WSD is Microsoft's discovery and scanning protocol. Some corporate/enterprise scanners only support WSD and not eSCL. The `sane-airscan` backend also supports WSD scanning.

### Packages
    sudo zypper install sane-backends sane-airscan simple-scan

### Verification
    scanimage -L          # List detected scanners
    # Should show something like:
    # device `airscan:e0:HP LaserJet MFP M234dw' is a eSCL HP LaserJet MFP M234dw ip=192.168.1.x

### Packages NOT Needed for Driverless Scanning
* No HPLIP (hp-scan)
* No iscan (Epson)
* No brscan (Brother)
* No vendor-specific SANE backends

Note: Some older scanners (pre-2015) may not support eSCL or WSD and will still require vendor-specific SANE backends. The distro should include `sane-backends` as a fallback but should not ship vendor bloatware by default.


---

## From legacy notes: Linux Printing (IPP Only).md
	•	Transport: IPP over HTTP
	•	Formats: PDF or Apple Raster
	•	Discovery: mDNS / Bonjour
Mopria is an industry consortium that standardized driverless IPP printing across vendors. It is effectively:
	•	IPP Everywhere + Mopria job-ticket profiles
	•	PDF / PCLm / JPEG raster
	•	mDNS or WSD discovery
	1.	V4 Print Driver Model
Modern Windows avoids vendor PCL/PS drivers and instead relies on class drivers.
	2.	Microsoft IPP Class Driver
This is the default for all network and USB network-function printers.
It conforms to:
	•	IPP Everywhere
	•	WSD/WS-Print (for discovery only; job transport increasingly IPP)
	3.	Windows supports IPP over USB, the same profile used on ChromeOS, iOS, and Linux.
What your distribution actually needs
If you provide:
	•	CUPS with full IPP Everywhere support
	•	mDNS/Bonjour discovery (Avahi)
	•	PDF and PWG/Apple Raster filters
	•	IPP-over-USB (ipp-usb)
…then you support:
	•	AirPrint
	•	Windows Modern Print (IPP Class Driver)
	•	ChromeOS
	•	Android
	•	All current enterprise MFPs that implement IPP Everywhere
No proprietary drivers are required unless a user insists on vendor-specific finishing or accounting extensions.
Here is the complete, minimal set for a modern driverless stack on openSUSE
Core printing: CUPS with IPP Everywhere
Install:
cups-filters
cups-filters-ipp
cups-filters-ghostscript
Discovery: AirPrint / Mopria
Install:
avahi-utils
Avahi provides Bonjour/mDNS service advertisements and discovery. AirPrint and Mopria depend on this.
IPP over USB (critical for modern printers)
Install:
This replaces usbbackend with a proper IPP service over a localhost port. All modern USB printers use this.
Packages you do NOT need
If your distro only supports IPP Everywhere:
	•	No HPLIP
	•	No Epson ESC/P-R
	•	No Canon UFR2
	•	No Brother LPR
	•	No foomatic-db
	•	No PPD packs
	•	No proprietary filters
