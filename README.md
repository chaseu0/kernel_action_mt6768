<div align="start">
  <h1>Kernel Build Action for Lancelot & Merlin</h1>
  <h3><i>Powered By GitHub Actions</i></h3>
</div>

A workflow to automatically build an Android kernel

## Usage Guide

> [!NOTE]
> You will need a GitHub account.

<details>
  <summary><i>Click to expand</i></summary>

> ### 1.
> ![01](guide/images/01.png)

> ### 2.
> ![02](guide/images/02.png)

> ### 3.
> ![03](guide/images/03.png)

> ### 4.
> ![04](guide/images/04.png)

> ### 5.
> ![05](guide/images/05.png)

> ### 6.
> ![06](guide/images/06.png)

> ### 7.
> ![07](guide/images/07.png)

> ### 8.
> ![08](guide/images/08.png)

> ### 9.
> ![09](guide/images/09.png)

> ### 10.
> ![10](guide/images/10.png)

> ### 11.
> ![11](guide/images/11.png)
> **Note:** Reload this page if the yellow circle does not appear.

> ### 12.
> ![12](guide/images/12.png)

> ### 13.
> ![13](guide/images/13.png)

> ### 14.
> ![14](guide/images/14.png)

</details>

## Options Guide

> [!NOTE]
> All options are located in [config.env](config.env)

| Base section options | Optional | Description | Example value |
|---------------------|----------|-------------|----------------|
| KERNEL_SOURCE | <div align="center">❌</div> | Link to kernel repository | `https://github.com/Jbub5/android_kernel_xiaomi_mt6768` |
| KERNEL_SOURCE_BRANCH | <div align="center">❌</div> | Branch name of kernel repository | <div align="center">`kernel-tree`</div> |
| KERNEL_ARCH | <div align="center">❌</div> | The Linux kernel architecture | <div align="center">`arm64`</div> |
| KERNEL_IMAGE_NAME | <div align="center">❌</div> | Format of the kernel image for flashable AnyKernel3 zip | <div align="center">`Image.gz-dtb`</div> |

<br>

| KernelSU section options | Optional | Description | Example value |
|--------------------------|----------|-------------|---------------|
| ENABLE_KERNELSU | <div align="center">✅</div> | Enables accounting for the KernelSU options written below | <div align="center">`true`</div> |
| KERNELSU_SETUP_SOURCE | <div align="center">❌</div> | Link to KernelSU setup script | `https://raw.githubusercontent.com/tiann/KernelSU/main/kernel/setup.sh` |
| KERNELSU_TAG | <div align="center">✅</div> | Repository branch or tag of KernelSU | <div align="center">`v1.0.1`</div> |
| ADD_KPROBES_CONFIG | <div align="center">✅</div> | Apply [patches](https://kernelsu.org/guide/how-to-integrate-for-non-gki.html#integrate-with-kprobe) kernel source code to support KernelSU installation via kprobe | <div align="center">`false`</div> |
| KSU_HOOKS_PATCH | <div align="center">✅</div> | Apply [patches](https://kernelsu.org/guide/how-to-integrate-for-non-gki.html#manually-modify-the-kernel-source) kernel source code to support KernelSU | <div align="center">`false`</div> |
| KSU_REVERT | <div align="center">✅</div> | Revert the [commit](https://github.com/tiann/KernelSU/commit/898e9d4f8ca9b2f46b0c6b36b80a872b5b88d899) that removed non-GKI kernel support | <div align="center">`true`</div> |
<!-- | KSU_EXPECTED_SIZE | idk | idk | idk | -->
<!-- | KSU_EXPECTED_HASH | idk | idk | idk | -->
> [!IMPORTANT]
> [KernelSU](https://kernelsu.org/guide/what-is-kernelsu.html) [no longer supports non-GKI kernels](https://github.com/tiann/KernelSU/issues/1705) after version [v1.0.0](https://github.com/tiann/KernelSU/releases/tag/v1.0.0). The last supported version is [v0.9.5](https://github.com/tiann/KernelSU/releases/tag/v0.9.5), please make sure to use the correct tag.

<br>

| APatch section options | Optional | Description | Example value |
|------------------------|----------|-------------|---------------|
| ADD_APATCH_SUPPORT | <div align="center">✅</div> | Add [required configs](https://apatch.dev/install.html#install-requirements) for [APatch](https://apatch.dev/what-is-apatch.html) and accounting for options written below | <div align="center">`false`</div> |
| FIX_APATCH_OPENELA | <div align="center">✅</div> | Apply fix for https://github.com/bmax121/APatch/issues/400 | <div align="center">`false`</div> |

<br>

| Compiler section options | Optional | Description | Example value |
|--------------------------|----------|-------------|---------------|
| USE_CLANG | <div align="center">✅</div> | Enable Clang toolchain usage and accounting for options written below | <div align="center">`true`</div> |
| CLANG_SOURCE | <div align="center">❌</div> | Link to clang archive or repository | `https://github.com/ZyCromerZ/Clang/releases/download/21.0.0git-20250415-release/Clang-21.0.0git-20250415.tar.gz` |
| CLANG_BRANCH | <div align="center">✅</div> | Branch name of clang repository | <div align="center">`main`</div> |
| USE_GCC | <div align="center">✅</div> | Enable GCC toolchain usage and accounting for options written below | <div align="center">`false`</div> |
| USE_GCC_64 | <div align="center">✅</div> | Enable GCC 64-bit | <div align="center">`true`</div> |
| GCC_64_SOURCE | <div align="center">❌</div> | Link to GCC 64-bit archive or repository | `https://snapshots.linaro.org/gnu-toolchain/14.0-2023.06-1/aarch64-linux-gnu/gcc-linaro-14.0.0-2023.06-x86_64_aarch64-linux-gnu.tar.xz` |
| GCC_64_BRANCH | <div align="center">✅</div> | Branch name of GCC 64-bit repository | <div align="center">`main`</div> |
| USE_GCC_32 | <div align="center">✅</div> | Enable GCC 32-bit | <div align="center">`true`</div> |
| GCC_32_SOURCE | <div align="center">❌</div> | Link to GCC 32-bit archive or repository | `https://snapshots.linaro.org/gnu-toolchain/14.0-2023.06-1/arm-linux-gnueabihf/gcc-linaro-14.0.0-2023.06-x86_64_arm-linux-gnueabihf.tar.xz` |
| GCC_32_BRANCH | <div align="center">✅</div> | Branch name of GCC 32-bit repository | <div align="center">`main`</div> |
> [!IMPORTANT]
> You can use only Clang or GCC, but not both at the same time.

<br>

| Anykernel3 section options | Optional | Description | Example value |
|----------------------------|----------|-------------|---------------|
| USE_ANYKERNEL3 | <div align="center">✅</div> | Enable creating flashable AnyKernel3 zip and accounting for options written below | <div align="center">`true`</div> |
| ANYKERNEL3_SOURCE | <div align="center">❌</div> | Link to AnyKernel3 source | `https://github.com/Jbub5/AnyKernel3.git` |
| ANYKERNEL3_BRANCH | <div align="center">✅</div> | Branch name of AnyKernel3 repository | <div align="center">`proton`</div> |

<br>

| Build section options | Optional | Description | Example value |
|-----------------------|----------|-------------|---------------|
| EXTRA_CMDS | <div align="center">✅</div> | Additional compiler options | `LLVM=1 LLVM_IAS=1 LD=ld.lld AS=llvm-as AR=llvm-ar NM=llvm-nm OBJCOPY=llvm-objcopy OBJDUMP=llvm-objdump READELF=llvm-readelf STRIP=llvm-strip CROSS_COMPILE=aarch64-linux-gnu- CROSS_COMPILE_ARM32=arm-linux-gnueabi- CROSS_COMPILE_COMPAT=arm-linux-gnueabi- CONFIG_NO_ERROR_ON_MISMATCH=y TARGET_BUILD_VARIANT=user` |
| WORKFLOW_DISABLE_LTO | <div align="center">✅</div> | Disables LTO by editing defconfig before build. Named with a workflow prefix to avoid colliding with the kernel's internal `DISABLE_LTO` make variable. | <div align="center">`false`</div> |
| DISABLE_CC_WERROR | <div align="center">✅</div> | Disable CONFIG_CC_WERROR | <div align="center">`false`</div> |
| ENABLE_PYTHON2 | <div align="center">✅</div> | Many old kernels require python2 to build them | <div align="center">`false`</div> |
| ENABLE_CCACHE | <div align="center">✅</div> | Enable [ccache](https://ccache.dev) | <div align="center">`false`</div> |
| REMOVE_UNUSED_PACKAGES| <div align="center">✅</div> | Remove unnecessary packages and free up more disk space for builder | <div align="center">`false`</div> |
| NEED_DTBO | <div align="center">✅</div> | Upload DTBO | <div align="center">`false`</div> |
| BUILD_BOOT_IMG | <div align="center">✅</div> | Repack boot.img by replacing the kernel in it with the compiled kernel | <div align="center">`false`</div> |
| SOURCE_BOOT_IMAGE | <div align="center">✅</div> | Link to the original boot.img for repackage it | `https://raw.githubusercontent.com/xiaoleGun/KernelSU_action/main/boot/boot.img` |

<br>

| Misc section options | Optional | Description | Example value |
|----------------------|----------|-------------|---------------|
| OLD_ANDROID_SUPPORT | <div align="center">✅</div> | Enable support for MIUI 12.5 and custom ROMs based on Android 11 through 12 | <div align="center">`false`</div> |
| ADD_OVERLAYFS_CONFIG | <div align="center">✅</div> | Automatically put the configs needed for OverlayFS into your defconfig | <div align="center">`false`</div> |
> [!IMPORTANT]
> Enabling OLD_ANDROID_SUPPORT breaks support for Android 13+.

<br>

| Proton specific section options | Optional | Description | Example value |
|----------------------------------|----------|-------------|---------------|
| FIX_WIFI_SPEED | <div align="center">✅</div> | Rollback to old connectivity drivers that have better Wi-Fi speed, but cause spontaneous reboots on some devices | <div align="center">`false`</div> |

## Credits

- [xiaoleGun](https://github.com/xiaoleGun) for base
- [dabao1955](https://github.com/dabao1955) for format of README.md
