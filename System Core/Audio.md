# Audio Subsystem

## Pipewire

PipeWire is the default audio/video server replacing both PulseAudio and JACK. It handles Bluetooth audio codecs, screen sharing (for Wayland), and pro-audio routing.

### Non-free Bluetooth Codecs
By default, PipeWire only includes open codecs (SBC, SBC-XQ). For high-quality Bluetooth audio, install non-free codec support:

    # OpenSUSE (from OBS pipewire-nonfree-codecs package — see OpenSUSE Configuration)
    sudo zypper install pipewire-nonfree-codecs

This enables:
* **AAC** — Required for Apple devices (AirPods, etc.). Licensing prevents inclusion in base repos.
* **aptX / aptX HD / aptX Adaptive** — Qualcomm's low-latency Bluetooth codecs. Common on Android devices and many Bluetooth headphones.
* **LDAC** — Sony's high-resolution Bluetooth codec. Note: LDAC is actually open-source (Apache 2.0) and may already be included in base PipeWire.
* **LC3/LC3plus** — Bluetooth LE Audio codec (part of the LE Audio/Auracast spec). This is the future standard and is royalty-free.

To verify active Bluetooth codec:
    pw-cli info all | grep -A 10 bluez

To check which codecs are available:
    spa-acp-tool list-codecs

[PipeWire Guide](https://github.com/mikeroyal/PipeWire-Guide)

## Realtime Audio Configuration

https://wiki.linuxaudio.org/wiki/system_configuration#audio_group

### Group Limits
User must be member of a group with sufficient rtprio and memlock set (e.g., audio or realtime):
    sudo usermod -a -G <group_name> <user_name>

### RT Priorities
Need to set up limits.conf for SCHED_FIFO with rtprio 80:
See https://wiki.linuxaudio.org/wiki/system_configuration#limitsconfaudioconf

### Power Management for Audio
Power management can't be controlled from user space; the device node /dev/cpu_dma_latency can't be accessed by the user. This prohibits DAWs like Ardour and Reaper from setting CPU DMA latency which could help prevent xruns.
See https://wiki.linuxaudio.org/wiki/system_configuration#quality_of_service_interface

### Swappiness for Audio
vm.swappiness=180 is too high for audio work. Set swappiness to 10 for audio production:
    vm.swappiness=10
See https://wiki.linuxaudio.org/wiki/system_configuration#sysctlconf

## Audio Enhancement (EasyEffects)

EasyEffects is a system-wide audio effects host for PipeWire (successor to PulseEffects). It applies a processing chain — limiter, auto-gain/loudness, dynamic-range compressor, 30-band parametric EQ, bass enhancer, exciter, crossfeed, reverb, delay, maximizer, and a **convolver** (impulse-response loading) — to output and input streams. The strong argument for shipping it by default: it does for laptop speakers what premium vendors (e.g. Apple) do in firmware — a generic community preset already produces a "massive" improvement, so users get good speaker sound with zero audio expertise.

Decision relevance for TC Benchtop Linux:
* **Ship candidate** — add `easyeffects` to the pattern (or as a Flatpak); packaged in Tumbleweed. VERIFY package name at merge.
* **Per-model laptop-speaker presets** in `tc-benchtop-settings` (Supported Models list) — a strong QoL differentiator for a laptop-targeted distro. JackHack96's "Advanced Auto Gain" is cited as a good generic default preset.
* The **convolver** (loading impulse responses / IRs) is the practical "Dolby Atmos alternative" for spatial/surround upmixing on Linux — see the linux_gaming thread below.

Reference links:
* EasyEffects should be part of every distro (laptop speaker quality): https://www.osnews.com/story/145883/easyeffects-should-be-part-of-every-linux-distribution-and-desktop-environment-to-massively-improve-laptop-speaker-sound-quality/
* PSA: EasyEffects can drastically improve audio: https://www.reddit.com/r/linux/comments/1laetsl/psa_easyeffects_can_drastically_improve_audio/
* Dolby Atmos alternative for Linux (spatial audio; convolver/IR-based approaches): https://www.reddit.com/r/linux_gaming/comments/1w2f441/dolby_atmos_alternative_for_linux/ — link filed from title; thread content not yet reviewed (Reddit blocks automated fetch)

## Audio Cues (UX Sound Design)

Design audio cues for:
* Any delayed response/action
* Drag and drop/file copy
* File download
* Empty trash
* Action not allowed (e.g., click outside box when input required)

Reference: https://utcc.utoronto.ca/~cks/space/blog/linux/SystemSoundsShouldBeGranular
