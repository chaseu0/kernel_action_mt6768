# Redmi 9 Kernel Rebuild for APatch

我把 `Redmi 9 (lancelot)` 这台机子的内核重新编出来了。

不是简单换 ROM，也不是拿现成镜像硬套，而是针对 `MT6768 / Android 11 / 4.14 非 GKI 内核`，把一整套可重复跑的 GitHub Actions 编译流水线整理出来，稳定产出：

- `Image.gz-dtb`
- `AnyKernel3` 可刷机包
- `build-metadata`
- `kallsyms-verification`
- `build.log`

这个仓库的核心目标很直接：

**给 Redmi 9 重编一个能走通 APatch 需求的内核，并且把整个流程工程化、自动化、可验证。**

---

## 我做成了什么

这不是“能跑起来就算了”的 demo 仓库，而是已经打通的 `kernel-only` 编译链路。

- 重新编译 `lancelot` 对应的 `mt6768` 内核
- 保留 `KernelSU` 兼容构建路线
- 单独做了 `KALLSYMS Verified` 工作流，专门面向 APatch
- 修掉了 `DISABLE_LTO` 和内核内部 make 变量冲突导致的 clang 构建崩溃
- 跑通了 `no-LTO` 路线，证明这份内核可以稳定完成完整编译
- 能输出带校验信息的 artifact，而不是只有一个“你自己猜能不能刷”的包

如果你知道 MTK 老内核、非 GKI、APatch、KALLSYMS、KernelSU 这些东西堆在一起有多难搞，你就会知道这套流水线不是玩具。

---

## 为什么这个项目值得写

`Redmi 9` 这类老 MTK 机型，最大的问题从来不是“能不能解锁”，而是：

- 内核老
- 非 GKI
- 符号表不完整
- APatch 对 `kallsyms` 有明确要求
- 社区现成包质量参差不齐
- 很多所谓“能用”的包根本没有把构建输入钉死

所以我没有走“下载一个别人打包好的神秘镜像然后祈祷”的路线。

我做的是：

**把 Redmi 9 的内核重编流程自己掌握下来。**

这意味着：

- 我知道它从哪个 kernel commit 编出来
- 我知道它用了哪个 KernelSU setup source
- 我知道它用了哪个 AnyKernel3 commit
- 我知道它的配置项到底有没有真的写进 `.config`
- 我知道 artifact 里到底是什么，而不是只看一个文件名自我感动

---

## 当前验证状态

### 1. 默认 `no-LTO` 内核编译已成功

成功 run：

- [27058699197](https://github.com/chaseu0/kernel_action_mt6768/actions/runs/27058699197)
- [27059782233](https://github.com/chaseu0/kernel_action_mt6768/actions/runs/27059782233)

这说明：

- `kernel-only` 路线已经跑通
- `WORKFLOW_DISABLE_LTO` 修复生效
- 不再出现 `clang: error: no such file or directory: 'true'`

### 2. APatch 目标配置已验证生效

成功 run：

- [27058656441](https://github.com/chaseu0/kernel_action_mt6768/actions/runs/27058656441)

这条 `KALLSYMS Verified` 路线确认了以下配置真实进入最终配置：

- `CONFIG_KALLSYMS=y`
- `CONFIG_KALLSYMS_ALL=y`
- `CONFIG_KALLSYMS_BASE_RELATIVE=y`
- `CONFIG_DEBUG_KERNEL=y`
- `# CONFIG_KALLSYMS_ABSOLUTE_PERCPU is not set`

这正是这台机器为 APatch 方向做内核准备时最关键的一步。

---

## 仓库里最重要的两条工作流

### `Build Kernel`

用途：

- 跑默认 `kernel-only` 编译
- 产出 `Image.gz-dtb` 和 `AnyKernel3`
- 适合验证工具链、编译稳定性、no-LTO 路线

配置文件：

- [`config.env`](config.env)

### `Build Kernel KALLSYMS Verified`

用途：

- 跑面向 APatch 的 `KALLSYMS` 验证编译
- 产出单独标记的 `KALLSYMS-VERIFIED` artifact
- 附带 `kallsyms-verification` 报告

配置文件：

- [`config-kallsyms-verified.env`](config-kallsyms-verified.env)

工作流文件：

- [`.github/workflows/build-kallsyms-verified.yml`](.github/workflows/build-kallsyms-verified.yml)

---

## 怎么跑流水线

### 跑默认内核编译

1. Fork 这个仓库
2. 打开 GitHub `Actions`
3. 选择 `Build Kernel`
4. 选择设备 `lancelot`
5. 运行 workflow
6. 下载产物：
   - `Image.gz-dtb-...`
   - `AnyKernel3-...`
   - `build-metadata-...`
   - `build-log-...`

### 跑 APatch / KALLSYMS 验证编译

1. 打开 GitHub `Actions`
2. 选择 `Build Kernel KALLSYMS Verified`
3. 直接运行
4. 下载产物：
   - `AnyKernel3-kallsyms-verified-...`
   - `Image.gz-dtb-kallsyms-verified-...`
   - `kallsyms-verification-...`
   - `build-metadata-...`

如果你真正关心的是 APatch，不要只跑默认工作流，直接跑 `KALLSYMS Verified`。

---

## 产物分别有什么用

### `Image.gz-dtb`

这是编译出来的原始内核镜像。

适合：

- 自己做二次封装
- 自己对比哈希
- 自己研究不同构建结果

### `AnyKernel3`

这是最适合刷入测试的 recovery 可刷机包。

适合：

- 快速验证新内核能不能上机
- 避免自己手工打包 boot image

### `build-metadata`

这里面有：

- `BUILD-INFO.txt`
- `SHA256SUMS.txt`
- raw `Image.gz-dtb`

适合：

- 对比不同 run 是否真的使用同一组输入
- 做哈希校验
- 复盘构建来源

### `kallsyms-verification`

这里面有：

- `kernel.config`
- `KALLSYMS-STATUS.txt`
- `SHA256SUMS.txt`

适合：

- 检查 `CONFIG_KALLSYMS_ALL` 是否真的打开
- 确认这次构建到底是不是 APatch 目标内核

---

## 这仓库和普通“内核模板仓库”的区别

很多仓库只是“能编一下”。

这仓库的重点是：

- 钉住关键输入
- 明确区分普通构建和 `KALLSYMS` 验证构建
- 输出构建日志和元数据
- 给 `Redmi 9 / lancelot` 这种老 MTK 机型做实战化内核工程

说得直白一点：

**这不是把脚本拼起来，这是把一台老 MTK 机器的内核编译链条真正吃透之后，整理成可以持续复用的流水线。**

---

## 适合谁

如果你是下面这几类人，这仓库就是为你准备的：

- 手里有 `Redmi 9 / lancelot`
- 想给这台机子上 APatch
- 不想盲刷来路不明的内核包
- 想自己掌控编译输入和输出
- 想把老 MTK 机型的内核构建做成工程，而不是一次性手工活

---

## 致谢

- [xiaoleGun](https://github.com/xiaoleGun) for base workflow ideas
- [Jbub5](https://github.com/Jbub5) for the mt6768-oriented kernel action foundation
- 所有还愿意折腾老 MTK 设备的人

---

## 一句话总结

**我不是在“改一个包”，我是把 Redmi 9 的内核重编、验证、打包、面向 APatch 的配置确认，整套链路都自己打通了。**
