# Security Architecture

## Principles
* Rolling release patches for fast security updates
* Limited attack surface:
    * Drop all GTK2, Python2
    * Simplified immutable core
* Userspace-only support for:
    * Legacy filesystems
    * File sharing protocols

## Kernel Security
* [KSPP](https://kspp.github.io/) — Kernel Self Protection Project
* [Kicksecure security-misc](https://www.kicksecure.com/wiki/Security-misc) / [GitHub](https://github.com/Kicksecure/security-misc)
* [Clear Linux Security](https://www.clearlinux.org/clear-linux-documentation/guides/clear/security.html)

## Firewalls

### Inbound
OpenSUSE uses firewalld by default (not ufw). firewalld is the better choice here — it integrates with NetworkManager, supports zones, and is actively maintained by Red Hat. ufw is Ubuntu-centric and would be a non-standard addition.

    sudo firewall-cmd --state                     # Check if running
    sudo firewall-cmd --get-default-zone          # Show default zone
    sudo firewall-cmd --list-all                  # Show current rules

For a GUI: `firewall-config` (GTK) or Cockpit's firewall panel.

### Outbound
[OpenSnitch](https://github.com/evilsocket/opensnitch) — Application-level outbound firewall (similar to Little Snitch on macOS). Prompts the user when an application tries to make an outbound connection for the first time. Very useful for detecting unexpected telemetry or data exfiltration.

## Rust Replacements for Security-Critical Tools
* https://discourse.ubuntu.com/t/carefully-but-purposefully-oxidising-ubuntu/56995
* https://www.phoronix.com/news/Ubuntu-25.10-sudo-rs-Default
* https://ubuntu.com/blog/tpm-backed-full-disk-encryption-is-coming-to-ubuntu

## Cryptography and Encryption

### Crypto Policies
* [Ubuntu crypto-config](https://github.com/canonical/crypto-config)
* [Fedora crypto-policies](https://gitlab.com/redhat-crypto/fedora-crypto-policies)
* [OpenSUSE crypto-policies](https://en.opensuse.org/SDB:Crypto-policies)

### Ideas
* fTPM Integration
* YubiKey Integration
* NitroKey Integration

### Disk Encryption
* LUKS Volume Encryption
* Cryptomator Cloud Encryption (encrypts individual files/folders for cloud storage — zero-knowledge encryption)
* CryptSetup Opal (hardware-based self-encrypting drive support via TCG Opal 2.0)

### systemd-cryptenroll
systemd-cryptenroll manages LUKS2 volume key enrollment. It supports:
* **TPM2 binding** — Automatically unlock LUKS volumes when TPM PCR values match (measured boot), without a passphrase
* **FIDO2 tokens** — Unlock with a YubiKey or other FIDO2 security key
* **PKCS#11 tokens** — Unlock with smart cards or hardware security modules
* **Recovery keys** — Generate and enroll a human-readable recovery key as a fallback

Usage examples:
    # Enroll TPM2 (auto-unlock when PCRs match):
    sudo systemd-cryptenroll --tpm2-device=auto --tpm2-pcrs=7+11 /dev/sdXn

    # Enroll a FIDO2 key:
    sudo systemd-cryptenroll --fido2-device=auto /dev/sdXn

    # Generate a recovery key:
    sudo systemd-cryptenroll --recovery-key /dev/sdXn

    # List enrolled keys:
    sudo systemd-cryptenroll /dev/sdXn

Note: TPM2 binding with PCR 7 (Secure Boot state) means the disk auto-unlocks only when the boot chain has not been tampered with. See TPM Issues section for known problems with PCR validation.

### TPM Issues

##### Tailscale
* https://news.ycombinator.com/item?id=46531925
* https://github.com/tailscale/tailscale/issues/17654
* https://github.com/tailscale/tailscale/issues/18288
* https://github.com/tailscale/tailscale/issues/18302

##### Aeon
* https://www.reddit.com/r/AeonDesktop/comments/1pwuvnw/pcr15_validation_again_unable_to_reenroll_please/
* https://www.reddit.com/r/AeonDesktop/comments/1o9wip0/the_validation_of_pcr_15_failed/

## polkit Configuration

OpenSUSE by default disallows wheel sudo; also sets Defaults targetpw. Need to change both settings and then lock root.

##### /polkit-1/rules.d/50-wheel-auth-self.rules
	/* /usr/share/polkit-1/rules.d/50-wheel-auth-self.rules */
	polkit.addRule(function(action, subject) {
	    if (subject.isInGroup("wheel")) {
	        return polkit.Result.AUTH_SELF;
	    }
	});

## Change to sudo Authentication (from targetpw)

OpenSUSE defaults to `Defaults targetpw` in sudoers, which means `sudo` asks for the **target user's** (root's) password rather than the invoking user's password. This is counterintuitive for users coming from Ubuntu/Fedora and a security risk (it means users must know the root password).

### Fix sudoers
    sudo visudo

Change:
    Defaults targetpw
To:
    # Defaults targetpw   (commented out)

And ensure the wheel group has sudo access:
    %wheel ALL=(ALL) ALL

### Lock the root account
After confirming sudo works with the user's own password:
    sudo passwd -l root

This prevents direct root login while still allowing `sudo` escalation via the wheel group.
