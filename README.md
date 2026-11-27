# zhitong-winbuild

ZHITONG Windows 版的构建仓库（**公开**，2026-11-27 起；私有仓库的免费额度不够用）。**这里只放工作流和说明，不放 ZHITONG 源码。**

## 怎么用

在 Mac 的 ZHITONG 目录运行：

```
tools/winbuild/run_ci.sh quick              # 构建 + 快测
tools/winbuild/run_ci.sh quick --skip-shell # 不编译 Tauri 外壳，只验证服务能起来（更快）
tools/winbuild/run_ci.sh full --keep-zip    # 全测，并把 zip 取回本机
```

脚本会：打源码快照 → 传到本仓库的**草稿 Release**（临时附件）→ 触发 `windows-build` 工作流（只能手动触发）→ 等结果 → 取回报告 → **删除草稿 Release**。产物（zip、报告）作为 Actions 附件只保留 1 天（公开仓库任何登录 GitHub 的人都能下载附件、看运行日志），脚本跑完会马上取回本机。

构建脚本本体在 ZHITONG 源码的 `winapp/build.ps1`、`winapp/ci/smoke.ps1`，随快照一起上传，所以这里不用同步改。

## 额度和费用

- 公开仓库用 GitHub 标准机器跑 Actions 免费、不计分钟。
- 约定不变：不加预算、不绑卡；任何地方提示付费或绑卡，立即停下告诉用户。

## 安全

- 不在工作流里打印任何凭据；只用 `GITHUB_TOKEN` 下载本仓库的草稿附件。
- 源码快照用完即删；不要把快照提交进仓库。
- 仓库是公开的：日志和附件外人能看，不要在工作流或测试里打印本机隐私、账号、真实令牌；测试数据一律随机生成。
