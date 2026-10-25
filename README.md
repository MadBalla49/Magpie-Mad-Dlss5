# Magpie-Mad-Dlss5
Magpie Mad Dlss5 Experimental This is a custom build of Magpie 0.6.11 Experimental
Magpie Mad Dlss5 Experimental
This is a custom build of Magpie 0.6.11 Experimental, rebuilt from source. It raises three DLSSNR strength limits, gives each Multi Pass layer its own working input resolution, and ships with the DLSSNR 310.8 runtime.

What’s new
1. Strength limits raised to 12
Setting	Original limit	This build
Shadow / Structure Strength	0–2	0–12
High-frequency Range Strength	0–2	0–12
Skin Structure Strength (Pass 1, 2, 3)	0–2	0–12
In the original, raising only the slider limit did nothing: Magpie’s code clamped these values back to 2 in two separate places. This build removes both caps, so the value you set is the value that’s applied. Shadow / Structure at 12 shows in the log as shadowStructureMultiplier=12.

2. Per-pass input resolution
DLSSNR · Pass 2 and DLSSNR · Pass 3 each get their own Input Resolution (%) slider (25–100%, default 100%).
The sliders show when Adjust Input Resolution is on. Changing one restarts the effect automatically.
Each pass runs NVIDIA’s DLSSNR at its own resolution, with its own motion data scaled to that size.
Where neighbouring passes differ in size, the image is resized on the GPU in between (anti-aliased Lanczos).
The final blend runs at the last pass’s resolution, so a sharp last pass keeps its detail. Put the highest resolution last, for example Pass 1 at 50% and Pass 2 at 100%.
The log shows the sizes, for example DLSSNR chain: passes=2 … intermediates=1280x720,2560x1440 exit=2560x1440.
Configs without per-pass values behave as before for Pass 1. A later pass with no value set uses its default of 100%.

3. DLSSNR runtime
Bundled: nvngx_dlssnr.dll 310.8, NVIDIA’s large network, chosen for its skin detail.
Kept as a backup: 310.9.16 as nvngx_dlssnr.dll.310.9.16.bak. It’s lighter because it uses a smaller network by default. To switch back, swap the file names with Magpie closed.
Included and removed features
Included: DLSS SR, DLSS Frame Generation, DLSSNR, FSR 3 upscaling and frame generation, AMD optical flow, XeSS SR and frame generation, RTX Video TrueHDR.
Not included: NVIDIA Optical Flow (NVOF), the NVIDIA VFX effects (including RTX Video denoise) and FSR 2. Their SDKs need an NVIDIA developer login or a community fork that wasn’t available.
Settings in this copy
“Dlss Pass 2” group: frame generation plus DLSSNR, Multi Pass 2, Pass 1 at 50%, Pass 2 at 100%, Skin Structure 12 on both passes, Shadow / Structure 12.
Always run as administrator is off; it can be turned back on in Settings.
Tested
Pass sizes 50%/33%, 33%/100% and 50%/100% were tested on moving test windows, and FF XVI ran six sessions. Every pass ran successfully with 0 errors.
Known issue (in the original build too): DLSS Frame Generation can crash with MP-061 if the source window stops sending new frames for a long time. If it happens, fully exit Magpie from the tray and reopen it. Using 2× frame generation or removing it avoids the crash.
Technical notes
Built with Visual Studio 2026 (v145 toolset), DLSS SDK 310.9.16, FSR SDK 2.3.0, XeSS SDK 3.0.1, NVAPI and RTX Video SDK 1.1. The version string is 0.6.11-experimental-custom.
Source changes: DLSSNRFilter.cpp, DLSSNRParameters.h, EffectParameterRules.h and DLSSNR_AI_Filter.hlsl, plus a local BuildOptions.props.user.
Backup: Magpie Mad Dlss5 Experimental.zip (680 MB) sits next to the folder.
