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
```bash
sudo dnf config-manager --add-repo https://cryfs-rpm.7234.cc.cd/cryfs.repo
sudo dnf install cryfs
# el9 若提示缺 FUSE 2 运行时，补一句：
# sudo dnf install fuse-libs
```

## 下载即用便携包（GitHub Releases，无需加仓库）
适合不想维护 yum 仓库、想丢个二进制就跑的场景（如宝塔上一键丢入）。

- `cryfs-1.0.3-linux-x86_64.tar.xz`（约 1.4MB）：解压后 `./cryfs --version`；首次需 `sudo dnf install -y fuse fuse-libs`（提供 `libfuse.so.2` + `fusermount`）。
- `cryfs-1.0.3-linux-x86_64-bundle-fuse.tar.xz`（约 1.5MB）：额外把 `libfuse.so.2` 打进 `lib/`，**挂载零安装**（卸载仍建议装 `fuse` 拿 `fusermount`）。
- 两者均含 `SHA256SUMS` + `verify.sh`（哈希自检），Releases 另附 `*.tar.xz.asc`（GPG 对 tarball 验签，指纹 `5926 7E53 EBD4 5672 122B 2CAF DA54 1775 AED8 69BF`）。
- 前提：内核需支持 FUSE（`/dev/fuse` 存在）；适用 el8/el9 x86_64，CentOS 7 / ARM / 无 FUSE 容器不适用。

## 信任
签名仅证明「持私钥者签过此包」，不证明构建可复现。指纹请以
**keyserver（keys.openpgp.org）+ 本页** 交叉核对为准；二者冲突时以 keyserver 为准。
