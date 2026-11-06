# cryfs-rpm

CryFS 1.0.3 签名 RPM 仓库，通过 GitHub Pages 分发。

- `centos8.5/`：仓库根（含 `repodata/` 与公钥 `RPM-GPG-KEY-cryfs-1.0.3.asc`）
- `fingerprint.html`：GPG 公钥指纹公示页（独立核对渠道之一）
- `cryfs.repo`：dnf/yum 客户端配置（baseurl 已指向本 Pages 站点）
- `FILES.md`：本仓库文件清单与逐项说明

## 兼容系统版本（实测，非推断）

| 系统 | 大版本 | 状态 | 说明 |
|---|---|---|---|
| CentOS 8.0–8.5 / Stream 8 / Rocky Linux 8 / AlmaLinux 8 | el8 | ✅ 原生 | glibc 2.28，产物最高要求即 2.28 |
| CentOS Stream 9 / Rocky Linux 9 / AlmaLinux 9 / RHEL 9 系 | el9 | ✅ 兼容 | glibc 2.34 向后兼容；安装时仅需补 `fuse-libs` |
| CentOS 7 / RHEL 7 | el7 | ❌ 不支持 | glibc 2.17 < 要求的 2.28，已 EOL |
| 非 RPM 系 / 非 x86_64 | — | ❌ 不适用 | 不交付 DEB 等 |

> **一个 RPM 通吃 el8 + el9**：产物（用 GCC 8.5.0 在 CentOS 8.5 编译）经 `readelf` 确认
> 最高只要求 `GLIBC_2.28`，因此 el8 全系原生可用，el9（glibc 2.34）向后兼容可直接装。
> 目录名 `centos8.5` 只是**构建基线**标签，不代表只支持 8.5。
> 完整证据见源项目 `docs/CentOS全系适配与GLIBC结论-2026-11-06.md`（含 Docker 真装日志）。

## 使用
\`\`\`bash
sudo dnf config-manager --add-repo https://cryfs-rpm.7234.cc.cd/cryfs.repo
sudo dnf install cryfs
# el9 若提示缺 FUSE 2 运行时，补一句：
# sudo dnf install fuse-libs
\`\`\`

## 信任
签名仅证明「持私钥者签过此包」，不证明构建可复现。指纹请以
**keyserver（keys.openpgp.org）+ 本页** 交叉核对为准；二者冲突时以 keyserver 为准。
