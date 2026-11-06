# 仓库文件清单与说明（FILES.md）

CryFS 1.0.3 签名 RPM 仓库，通过 GitHub Pages 分发。
覆盖范围：**el8 全系（CentOS 8.0–8.5 / Stream 8 / Rocky 8 / Alma 8）与 el9 全系（Rocky 9 / Alma 9 / RHEL 9 / Stream 9）**，均为 x86_64。
同一 `centos8.5/` 目录同时服务这两大类系统（产物最高只要求 `GLIBC_2.28`，el9 的 2.34 向后兼容）。
**不支持** el7 及更早（glibc 2.17 < 2.28，且已 EOL）与非 RPM / 非 x86_64 平台。

---

## 一、顶层文件

| 文件 | 大小 | 说明 |
|---|---|---|
| `README.md` | 1.0K | 仓库总说明：用途、客户端使用命令、信任模型（同站页与 keyserver 冲突以 keyserver 为准）。 |
| `index.html` | 8.0K | **站点首页**（GitHub Pages 自定义域名服务在根路径，裸域名 `cryfs-rpm.7234.cc.cd/` 直接访问返回本页，解决裸域名 404）。含一键安装命令、指纹核对指引、兼容矩阵。 |
| `cryfs.repo` | 1.0K | dnf/yum 客户端配置。`baseurl`/`gpgkey` 指向本站点根路径；`gpgcheck=1` 保留（包级签名已签），`repo_gpgcheck=0`（未对 repomd.xml 单独签名）。 |
| `fingerprint.html` | 8.0K | GPG 公钥指纹公示页（独立核对渠道之一），含完整指纹 `5926 7E53 … AED8 69BF`、Key ID、UID、有效期。 |
| `.nojekyll` | 0 | 告诉 GitHub Pages 不要用 Jekyll 处理站点，避免 RPM / 二进制被忽略或错误处理。 |
| `.gitignore` | 1.0K | 忽略本地 `PUSH.txt` 备忘，防止误入公开仓库。 |

---

## 二、`centos8.5/` 仓库根目录

| 文件 | 大小 | 说明 |
|---|---|---|
| `RPM-GPG-KEY-cryfs-1.0.3.asc` | 8.0K | 仓库 **GPG 公钥**。用户导入后 `dnf` 才能 `gpgcheck=1` 自动验签。 |
| `cryfs-1.0.3-Linux-signed.rpm` | 1.8M | **主交付物**：GPG 签名后的 CryFS 1.0.3 RPM，dnf 直接安装的就是它。 |
| `cryfs-1.0.3-Linux.rpm` | 1.8M | 同一 RPM 的**未签名**副本。透明保留，便于高级用户手动校验或自行本地重签名。 |
| `SHA256SUMS` | 1.0K | 仓库内文件 SHA-256 清单（明文），供离线/独立核对完整性。 |
| `SHA256SUMS-signed` | 1.0K | 上述清单的 GPG 签名，提供「清单本身未被篡改」的强校验（可选）。 |
| `UNSIGNED.md` | 8.0K | 透明披露：说明未签名 RPM 的存在、为何保留、以及对应风险与适用场景。 |

> 签名 RPM 与未签名 RPM 体积相同属正常——RPM V4 签名写入包头而非追加尾部，不改变载荷总大小。

---

## 三、`centos8.5/repodata/` 仓库元数据

由 `createrepo_c` 生成，是 dnf/yum 消费本仓库的索引层。

| 文件 | 大小 | 说明 |
|---|---|---|
| `repomd.xml` | 8.0K | 元数据总索引（dnf 的入口），列出下面各元数据文件的校验与位置。 |
| `*-primary.xml.gz` | 1.0K | 包主元数据：包名、版本、架构、依赖、摘要。 |
| `*-primary.sqlite.bz2` | 8.0K | 同上内容的 SQLite 二进制版，dnf 本地缓存用。 |
| `*-filelists.xml.gz` | 1.0K | 文件清单元数据：每个包安装了哪些文件。 |
| `*-filelists.sqlite.bz2` | 8.0K | 同上内容的 SQLite 二进制版。 |
| `*-other.xml.gz` | 1.0K | 其他元数据：变更日志、安装/卸载脚本等。 |
| `*-other.sqlite.bz2` | 8.0K | 同上内容的 SQLite 二进制版。 |

---

## 四、最小可用集合（用户真正需要的）

要让一台干净 el8.5/el9 机器装上并验签，只需这 4 类：

1. `cryfs.repo` —— 告诉 dnf 去哪、公钥在哪、要验签
2. `centos8.5/RPM-GPG-KEY-cryfs-1.0.3.asc` —— 验签用的公钥
3. `centos8.5/cryfs-1.0.3-Linux-signed.rpm` —— 包本体
4. `centos8.5/repodata/` —— 让 dnf 能识别这是个仓库

其余（`fingerprint.html`、`SHA256SUMS*`、`UNSIGNED.md`、未签名 RPM）均为**透明/可选**的信任增强与审计材料，缺失不影响安装，但建议在审计场景下核对。
