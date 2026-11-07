# KernelSU Patches for Samsung Galaxy A9 (2018) / SDM660

This repository stores KernelSU / KernelSU-Next manual hook patches and opt-in configurations for kernel trees that remain plain and clean in git.

## Structure

```
ksu_patches/
└── 4.19/
    └── 0001-KernelSU-Next-manual-hooks.patch
```

## Usage

### Linux 4.19 (`android_kernel_samsung_a9y18qlte-4.19`)

1. Apply the patch to the kernel source directory:
   ```bash
   git -C kernel/samsung/a9y18qlte-4.19 apply /path/to/ksu_patches/4.19/0001-KernelSU-Next-manual-hooks.patch
   ```

2. Clone KernelSU-Next (`legacy` branch) and link the driver:
   ```bash
   git clone --depth=1 -b legacy https://github.com/KernelSU-Next/KernelSU-Next kernel/samsung/a9y18qlte-4.19/KernelSU-Next
   ln -sfn ../KernelSU-Next/kernel kernel/samsung/a9y18qlte-4.19/drivers/kernelsu
   ```

3. Build the kernel with `vendor/ksu.config` enabled (or `TARGET_KERNEL_KSU=true`).

4. To restore the kernel tree back to its plain state:
   ```bash
   git -C kernel/samsung/a9y18qlte-4.19 checkout .
   git -C kernel/samsung/a9y18qlte-4.19 clean -fd
   rm -rf kernel/samsung/a9y18qlte-4.19/KernelSU-Next kernel/samsung/a9y18qlte-4.19/drivers/kernelsu
   ```
