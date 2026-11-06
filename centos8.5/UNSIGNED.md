# UNSIGNED · 未签名基线标记

> 这是**标记文件**，用于明确区分未签名原件与签名版。它本身不是 RPM、不参与任何校验清单。
> 本目录的 `cryfs-1.0.3-Linux.rpm` 是 **2026-11-06 编译产出的未签名原始基线**，按要求**保留原件、字节与文件名均未改动**。

| 文件 | 状态 | SHA-256 |
|---|---|---|
| `cryfs-1.0.3-Linux.rpm` | **未签名基线**（`Signature : (none)`，保留原件） | `ec8a317e7c4fdc1809a550f2e935020defaef48db069cfe12a60b94ebcc183a0` |
| `cryfs-1.0.3-Linux-signed.rpm` | **已签名版**（推荐对外分发） | `b0e34f262e5478af4725b9bd258cbab8bc36211d8a6df4a086b061c0e31fdf3d` |

- 构建环境：CentOS Linux release **8.5.2111** / GCC 8.5.0
- 基线清单 `SHA256SUMS`：只含未签名原件，签名后**未改动**（`cd` 到本目录后 `sha256sum -c SHA256SUMS`）
- 签名清单 `SHA256SUMS-signed`：只含签名版（`sha256sum -c SHA256SUMS-signed`）
- 公钥：`../RPM-GPG-KEY-cryfs-1.0.3.asc`，指纹 `59267E53EBD45672122B2CAFDA541775AED869BF`，Key ID `da541775aed869bf`
- 签名算法：RSA/SHA256（RPM Header V4），签名时间 `Thu Nov  5 21:05:19 2026 UTC`（= GMT+8 11-06 05:05）

## 为什么保留未签名基线

1. **可追溯**：保留「编译 → 签名」前后两套哈希，任何一步都能单独复验。
2. **引用不变**：已有文档、脚本、教程里的文件名与哈希继续有效，不必全局改引用。
3. **负向对照**：`rpm -Kv` 未签名基线只有 4 项摘要、无 RSA 签名行，可用来确认验签工具真的在区分签名状态。

## 校验与验签

```bash
cd rpm/centos8.5
sha256sum -c SHA256SUMS            # 未签名基线
sha256sum -c SHA256SUMS-signed     # 签名版

# 目标机（导入公钥后验签）
sudo rpm --import ../RPM-GPG-KEY-cryfs-1.0.3.asc
rpm -Kv cryfs-1.0.3-Linux-signed.rpm
# 期望出现：Header V4 RSA/SHA256 Signature, key ID aed869bf: OK
```

> 未签名基线**并非没有完整性保护**：`rpm -Kv` 的 Header SHA256 / Header SHA1 / Payload SHA256 / MD5 四项摘要均 OK，
> 能检出传输损坏；缺的只是「来源认证」。分发请优先用 `*-signed.rpm` 并随附公钥。
