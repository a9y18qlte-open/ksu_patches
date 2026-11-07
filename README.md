# KernelSU & BakaSU Patches for Samsung Galaxy A9 (2018) / SDM660

This repository stores KernelSU / BakaSU manual hook patches, SUSFS backports, and opt-in configurations for kernel trees that remain plain and clean in git.

## Repository Structure

```
ksu_patches/
├── 4.4/
│   ├── 0001-BakaSU-and-SUSFS-v2.3.0-4.4.patch
│   └── 0002-BakaSU-preinit-runner-aarch64-target.patch
├── 4.19/
│   └── 0001-KernelSU-Next-manual-hooks.patch
└── README.md
```

---

## Linux 4.4 (`android_kernel_samsung_a9y18qlte`) — BakaSU + SUSFS v2.3.0

1. **Apply manual hooks and SUSFS v2.3.0 to the kernel:**
   ```bash
   git -C <kernel-dir> apply /path/to/ksu_patches/4.4/0001-BakaSU-and-SUSFS-v2.3.0-4.4.patch
   ```

2. **Setup BakaSU driver and apply AArch64 preinit runner patch:**
   ```bash
   git clone --depth=1 https://github.com/Baka-SU/BakaSU <kernel-dir>/KernelSU
   # Upstream BakaSU preinit runner makefile needs AArch64 target flag on 4.4 Clang:
   git -C <kernel-dir>/KernelSU apply /path/to/ksu_patches/4.4/0002-BakaSU-preinit-runner-aarch64-target.patch
   ln -sfn ../KernelSU/kernel <kernel-dir>/drivers/kernelsu
   ```

3. **Build the kernel:**
   The patch provides `arch/arm64/configs/vendor/ksu.config` enabling `CONFIG_KSU=y`, `CONFIG_KSU_SUSFS=y`, and all SUSFS sub-options.
   Build with `TARGET_KERNEL_KSU=true m bootimage`.

---

## Linux 4.19 (`android_kernel_samsung_a9y18qlte-4.19`) — KernelSU-Next

1. **Apply manual hooks to the kernel:**
   ```bash
   git -C <kernel-dir> apply /path/to/ksu_patches/4.19/0001-KernelSU-Next-manual-hooks.patch
   ```

2. **Setup KernelSU-Next driver:**
   ```bash
   git clone --depth=1 -b legacy https://github.com/KernelSU-Next/KernelSU-Next <kernel-dir>/KernelSU-Next
   ln -sfn ../KernelSU-Next/kernel <kernel-dir>/drivers/kernelsu
   ```

3. **Build the kernel:**
   Append `vendor/ksu.config` to defconfig (e.g. `make ... vendor/sdm660-perf_defconfig vendor/a9y18qlte.config vendor/ksu.config`).
