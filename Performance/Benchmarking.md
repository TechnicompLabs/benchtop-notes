# Benchmarking

## Latency (Desktop Responsiveness)

Goal: Measure that the desktop remains responsive under heavy background load. This is the primary differentiator for a well-tuned desktop distro.

Measure GUI FPS and input latency under:
* Large file copies (e.g., 50GB dd or rsync between disks)
* Video encoding script (e.g., ffmpeg x265 encoding, all cores)
* All-core compile (e.g., Linux kernel `make -j$(nproc)`)
* Heavy memory pressure (e.g., stress-ng --vm 4 --vm-bytes 80%)
* Concurrent disk + CPU + memory stress

Tools:
* Frame timing: `libframetime` or MangoHUD's frametime logging in any Vulkan/GL app
* Input latency: `evhz` (mouse poll rate), `wev` (Wayland event latency)
* Compositor latency: GNOME's built-in frame clock (`MUTTER_DEBUG=1`), or record a 240fps video of input-to-display
* General system latency: `cyclictest` (from rt-tests package) — measures scheduling latency
* Desktop stutter detection: `intel_gpu_top` or `nvtop` for GPU frame drops during compositing

Test procedure:
1. Baseline: Measure idle desktop framerate and input latency
2. Load: Start background workload (see list above)
3. Interaction test: Open/close apps, switch windows, scroll web pages, drag windows, play video
4. Record: Frame times, scheduling latency percentiles (p50, p95, p99, max)

Target: p99 scheduling latency < 1ms, no visible stutter during window management under full CPU load

## Throughput

Goal: Verify that desktop tuning does not significantly regress raw computational performance.

Benchmarks:
* Kernel compile time: `time make -j$(nproc)` on a consistent kernel tree
* File I/O: `fio` with typical desktop patterns (4K random read/write, sequential read/write)
* Network: `iperf3` for TCP/UDP throughput
* Memory bandwidth: `stream` benchmark (STREAM Triad)
* Compression: `zstd` or `lz4` compression/decompression throughput
* General: Phoronix Test Suite (`phoronix-test-suite benchmark pts/build-linux-kernel`)

Compare against:
* Vanilla kernel (same version, default config)
* CachyOS defaults
* Clear Linux defaults
* Fedora Workstation defaults

## Gaming

Goal: Ensure gaming performance is competitive with dedicated gaming distros (CachyOS, Nobara).

Benchmarks:
* Frame times (not just average FPS — 1% lows and 0.1% lows matter more for perceived smoothness)
* Use MangoHUD to log frame times: `MANGOHUD=1 MANGOHUD_LOG=1 <game>`
* Test with GameMode enabled vs. disabled
* Test with THP always vs. madvise

Suggested test titles:
* A Vulkan-native Linux game (e.g., CS2 on Linux)
* A Proton/Wine game (e.g., Cyberpunk 2077 via Proton)
* An OpenGL game (e.g., Minecraft with shaders)
* A GPU-compute workload (e.g., Blender Cycles render)

Metrics:
* Average FPS
* 1% low FPS
* 0.1% low FPS
* Frame time variance (std dev)
* CPU scheduling latency during gameplay (cyclictest in background)

Tools:
* MangoHUD (overlay + logging)
* vkbasalt (post-processing, for visual quality testing)
* Phoronix Test Suite gaming benchmarks
* `gamemode -s` to verify GameMode is active


---

## From legacy notes: Benchmarking.md

# Latency
Measure GUI FPS Under
* Large file copies
* Video encoding script
* All core compile
