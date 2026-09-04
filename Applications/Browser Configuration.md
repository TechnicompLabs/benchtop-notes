# Browser Configuration

## Brave (Default Browser)

Custom policy disabling unnecessary features:
    "BraveRewardsDisabled": true,
    "BraveWalletDisabled": true,
    "BraveVPNDisabled": 1,
    "BraveAIChatEnabled": false,
    "TorDisabled": true,
    "DnsOverHttpsMode": "automatic"

## Firefox

### Fix Scrolling
    apz.gtk.pangesture.delta_mode=2
    apz.gtk.pangesture.pixel_delta_mode_multiplier=25
[Bug](https://bugzilla.mozilla.org/show_bug.cgi?id=1752862)

### Switch Browser Cache from Disk (SSD) to RAM
    browser.cache.disk.enable=false
    browser.cache.disk_cache_ssl=false

### Font Selection
#TODO

### Browser Cache
#TODO — Investigate optimal cache settings


---

## From legacy notes: Brave.md
- Brave is now the default browser. We ship it with a custom policy that disables the following:
    “BraveRewardsDisabled”: true,
    “BraveWalletDisabled”: true,
    “BraveVPNDisabled”: 1,
    “BraveAIChatEnabled”: false,
    “TorDisabled”: true,
    “DnsOverHttpsMode”: “automatic”


---

## From legacy notes: Firefox.md

## Switch browser cache from disk (SSD) to RAM
    browser.cache.disk.enable=false # Change from **true** to **false**
    browser.cache.disk_cache_ssl=false from **true** to **false**
