# zhitong-winbuild

ZHITONG Windows 版的构建仓库（私有）。**这里只放工作流和说明，不放 ZHITONG 源码。**

## 怎么用

在 Mac 的 ZHITONG 目录运行：

```
tools/winbuild/run_ci.sh quick              # 构建 + 快测
tools/winbuild/run_ci.sh quick --skip-shell # 不编译 Tauri 外壳，只验证服务能起来（更快）
tools/winbuild/run_ci.sh full --keep-zip    # 全测，并把 zip 取回本机
```

脚本会：打源码快照 → 传到本仓库的**草稿 Release**（临时附件）→ 触发 `windows-build` 工作流（只能手动触发）→ 等结果 → 取回报告 → **删除草稿 Release**。产物（zip、报告）作为 Actions 附件只保留 3 天。

构建脚本本体在 ZHITONG 源码的 `winapp/build.ps1`、`winapp/ci/smoke.ps1`，随快照一起上传，所以这里不用同步改。

## 额度和费用

- 私有仓库的 Windows 机器按 0.010 美元/分钟计，GitHub Free 每月含 12 美元计算额度（约 1200 分钟 Windows），和本账号其他私有仓库共用。
- **账号的付款方式没有核实**（本机 `gh` 没有 `user` 权限，查不到账单）。约定：**用完免费额度就停**，不加预算、不绑卡；任何地方提示付费或绑卡，立即停下告诉用户。
- 每次运行 `run_ci.sh` 会打印本次计费分钟，记到 ZHITONG 的 `docs/Windows版_构建记录.md`。
- 平时跑 `quick`；`full` 按需。一次完整构建目标 25 分钟以内。

## 安全

- 不在工作流里打印任何凭据；只用 `GITHUB_TOKEN` 下载本仓库的草稿附件。
- 源码快照用完即删；不要把快照提交进仓库。
