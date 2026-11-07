# KernelSU Patches for Samsung Galaxy A9 (2018) / SDM660

This repository stores KernelSU / KernelSU-Next manual hook patches and opt-in configurations for kernel trees that remain plain and clean in git.

## Repository Structure

```
ksu_patches/
├── 4.4/
│   ├── 0001-KernelSU-Next-manual-hooks.patch
│   └── 0002-KernelSU-Next-4.4-compat.patch
├── 4.19/
│   └── 0001-KernelSU-Next-manual-hooks.patch
└── README.md
```

---

## Linux 4.19 (`android_kernel_samsung_a9y18qlte-4.19`)

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

---

## Linux 4.4 (`android_kernel_samsung_a9y18qlte`)

1. **Apply manual hooks to the kernel:**
   ```bash
   git -C <kernel-dir> apply /path/to/ksu_patches/4.4/0001-KernelSU-Next-manual-hooks.patch
   ```

2. **Setup KernelSU-Next driver and apply 4.4 compat patch:**
   ```bash
   git clone --depth=1 -b legacy https://github.com/KernelSU-Next/KernelSU-Next <kernel-dir>/KernelSU-Next
   # Upstream KernelSU-Next legacy needs 4.4 compiler/header fixes:
   git -C <kernel-dir>/KernelSU-Next apply /path/to/ksu_patches/4.4/0002-KernelSU-Next-4.4-compat.patch
   ln -sfn ../KernelSU-Next/kernel <kernel-dir>/drivers/kernelsu
   ```

3. **Build the kernel:**
   Append `vendor/ksu.config` to defconfig (e.g. `make ... a9y18qlte_eur_open_defconfig vendor/ksu.config`).

---

## Note on KernelSU-Next `setup.sh`

Running:
```bash
curl -LSs "https://raw.githubusercontent.com/KernelSU-Next/KernelSU-Next/next/kernel/setup.sh" | bash -s legacy
```
- Passes argument `legacy` to `setup.sh`, which runs `git checkout legacy`.
- Checks out the **latest commit on the `legacy` branch** (HEAD of `legacy`), **not** a tag. (Only running `setup.sh` without arguments checks out the latest tag).
- `setup.sh` only configures `drivers/` (driver symlink and driver Makefile/Kconfig); it does **not** install manual hooks in core kernel subsystems (`fs/`, `drivers/input/`, `kernel/reboot.c`, `security/selinux/`, `include/linux/seccomp.h`) which are required on non-GKI kernels.
