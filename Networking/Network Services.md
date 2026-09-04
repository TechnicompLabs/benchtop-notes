# Network Services

## Avahi (mDNS / Bonjour)
    sudo systemctl enable --now avahi-daemon

## Hostname Resolution
##### /etc/nsswitch.conf.d/10-hostname.conf
Change line `hosts: files mdns_minimal [NOTFOUND=return] dns` to:
	hosts:      files myhostname mdns_minimal [NOTFOUND=return] dns

## Firewall Configuration
    firewall-cmd --get-active-zones
    firewall-cmd --permanent --zone=home --change-interface=<interface>
    sudo firewall-cmd --reload

    sudo firewall-cmd --permanent --zone=home --add-service=ssh
    sudo firewall-cmd --permanent --zone=home --add-service=mdns
    sudo firewall-cmd --permanent --zone=home --add-service=cockpit

## File Sharing
* SMB3 File Sharing (considering whether to target lowest common denominator or highest)
* SSHFS/MOSHFS atomicity
* Syncthing text file configuration
* Tailscale with headscale
* Nextcloud?
* Localsend (AirDrop equivalent)

## Remote Management
### Cockpit
    zypper in patterns-microos-cockpit cockpit-ws cockpit-tukit cockpit-machines
    systemctl enable --now cockpit.socket

## Mac OS X Network Sharing Services (Reference for Feature Parity)
### Content and Media
* File Sharing
* Media Sharing
* Screen Sharing
* Content Caching

### Accessories and Internet
* Bluetooth Sharing
* Printer Sharing
* Web Sharing

### Advanced
* Remote Management
* Remote Login
* Remote Application Scripting

### Other
* Target Disk Mode

### Deprecated
* Airplay Receiver, Xgrid Sharing, Web Sharing, DVD/CD Sharing


---

## From legacy notes: Network Services.md

# Security
Inbound Firewall: ufw
Outbound Firewall: OpenSnitch

## Content and Media
File Sharing: SSHFS
Screen Sharing: RustDesk

## Advanced
Remote Management: Cockpit
Remote Login: Web
Bitwarden
Tailscale

### Content and Media
File Sharing
Media Sharing
Screen Sharing
Content Caching

### Accessories and Internet
Bluethooth Sharing
Printer Sharing
Web Sharing

### Advanced
Remote Management
Remote Login
Remote Application Scripting

### Other
Target Disk Mode

### Deprecated:
Airplay Receiver
Xgrid Sharing
Web Sharing
DVD or CD Sharing
