# cryfs-rpm

CryFS 1.0.3 签名 RPM 仓库（el8.5 + el9），通过 GitHub Pages 分发。

- `centos8.5/`：仓库根（含 `repodata/` 与公钥 `RPM-GPG-KEY-cryfs-1.0.3.asc`）
- `fingerprint.html`：GPG 公钥指纹公示页（独立核对渠道之一）
- `cryfs.repo`：dnf/yum 客户端配置（baseurl 已指向本 Pages 站点）

## 使用
\`\`\`bash
sudo dnf config-manager --add-repo https://<你的GitHub用户名>.github.io/cryfs-rpm/cryfs.repo
sudo dnf install cryfs
\`\`\`

## 信任
签名仅证明「持私钥者签过此包」，不证明构建可复现。指纹请以
**keyserver（keys.openpgp.org）+ 本页** 交叉核对为准；二者冲突时以 keyserver 为准。
