---
{"dg-publish":true,"permalink":"/wiki/tech-and-engineering/devices/offload-rendering-to-thunderbolt-gpu/","title":"Offload rendering to Thunderbolt GPU","tags":["Linux","Guide","Electronics","Gaming"],"dg-note-properties":{"type":"Page","collections":"Knowledge & Guides","title":"Offload rendering to Thunderbolt GPU","aliases":null,"description":"Guidelines for using PRIME Render Offload to run GPU-intensive applications on an external NVIDIA Thunderbolt eGPU while displaying output on the internal GPU in Linux.","icon":null,"createdAt":"2025-11-29T08:55:06.884Z","lastUpdated":"2026-02-25T16:05:20.819Z","tags":["Linux","Guide","Electronics","Gaming"],"coverImage":null}}
---

# Offload rendering to Thunderbolt GPU

## Offloading rendering to a Thunderbolt eGPU (NVIDIA)
> Use PRIME Render Offload to render on an external NVIDIA GPU connected via Thunderbolt, while displaying on the internal screen.

### Goal
Render apps on the external NVIDIA GPU and display the frames on the integrated GPU without switching the whole X session to NVIDIA.

### Prerequisites
- Linux with Xorg or Wayland session that supports PRIME Render Offload
- NVIDIA proprietary driver installed and loaded
- nvidia-prime or PRIME offload support available in the driver stack
- Thunderbolt eGPU recognized by the system (`lspci | grep -i nvidia`)

### Quick command (one-off)

Run an app on the eGPU with PRIME offload:
```bash
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia <your_command>
```

Examples:
```bash
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia glxinfo -B
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia firefox
```

On Wayland with Vulkan apps, add the Vulkan layer variable:
```bash
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only <your_command>
```

### Verify it is using the eGPU
- OpenGL: `glxinfo -B` should show `OpenGL renderer string: NVIDIA ...`
- Vulkan: `vulkaninfo | grep -A2 "GPU id"` and confirm the selected device is NVIDIA
- NVIDIA state: `nvidia-smi` should list the process under Processes

### Optional: create a helper
Add a small wrapper to run anything on the eGPU.

Create `~/bin/egpu-run` and make it executable:
```bash
mkdir -p ~/bin
cat > ~/bin/egpu-run <<'EOF'
#!/usr/bin/env bash
export __NV_PRIME_RENDER_OFFLOAD=1
export __GLX_VENDOR_LIBRARY_NAME=nvidia
export __VK_LAYER_NV_optimus=NVIDIA_only
exec "$@"
EOF
chmod +x ~/bin/egpu-run
```

Usage:
```bash
egpu-run blender
egpu-run steam
```

### Troubleshooting
- Driver not loaded: `lsmod | grep nvidia` should show modules. Reinstall or load the driver if missing.
- Library vendor mismatch: ensure `__GLX_VENDOR_LIBRARY_NAME=nvidia` is set for OpenGL apps.
- Wayland session: some compositors restrict offloading for XWayland or native Wayland apps. Try an Xorg session to compare.
- PRIME unavailable: check `nvidia-settings` under PRIME Profiles for Offloading support, or inspect `/var/log/Xorg.0.log` for PRIME lines.
- No processes in `nvidia-smi`: the app may be CPU-bound or rendering on the iGPU. Re-run with the env vars and verify via `glxinfo -B`.
- Thunderbolt permissions: some distros require authorizing the eGPU in the firmware or via boltctl: `boltctl authorize <UUID>`.

### Notes
- This offloads rendering per-application. It does not switch the whole desktop to NVIDIA.
- Performance depends on the Thunderbolt bandwidth and the app's rendering path.

