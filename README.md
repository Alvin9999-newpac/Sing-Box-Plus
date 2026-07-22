# 🚀 Sing-Box-Plus 一键管理脚本（20 节点：直连 10 + WARP 10）

开箱即用 20 个节点（直连 10 + WARP 10），含端口一键切换、BBR 加速、分享链接导出等。

* ✅ 已适配 **sing-box v1.13.x**（已固定为 v1.13.7）
* ✅ 支持 **WARP 出站**（官方 warp-cli，更高兼容性，非 wgcf）
* ✅ 一键生成证书（自签），一键 systemd 托管
* ✅ **更换端口**后自动重写配置与放行
* ✅ 分享链接分组打印（直连 10 / WARP 10），导入即用
* ✅ WARP 节点，将服务器 IP "变身" 为 Cloudflare 的中性出口，Gemini/Netflix/Disney+/YouTube 等流媒体解锁
* ✅ **新增 AnyTLS 协议**（直连 + WARP 各一个），抗流量分析能力更强

**🔔 2026年7月22日更新：** 修复hysteria2的2个pinnedPeerCertSha256节点链接，适配 Xray-core v26.2.6+ 移除 allowInsecure 后旧节点无法启动的问题（自 2026-06-01 起生效）。

---

## ✨ 默认部署内容（20 个节点）

**直连 10：**

* VLESS Reality（Vision 流）
* VLESS gRPC Reality
* Trojan Reality
* VMess WS
* Hysteria2（直连证书）
* Hysteria2 + OBFS(salamander)
* Shadowsocks 2022（2022-blake3-aes-256-gcm）
* Shadowsocks（aes-256-gcm）
* TUIC v5（ALPN h3，自签证书）
* **AnyTLS**（自签证书，sing-box v1.12+ 引入的新协议，抗流量分析）

**WARP 10：**（同上 10 种，出站经 Cloudflare WARP）

> WARP 出站更利于流媒体解锁与回程质量。

**注意：Shadowsocks 2022 和 Shadowsocks 协议可能容易被封，不推荐使用。**

**关于 AnyTLS：** 这是 sing-box v1.12 起加入的新协议，使用标准 TLS 流量伪装，并通过 Padding 抵抗流量分析。客户端需要 sing-box 1.12+、Mihomo (Clash.Meta) 较新版本、Hiddify 较新版本，老版本 v2rayN/Shadowrocket 可能不支持。

---

## ✅ 支持系统

 - Debian 11+ ✅
 - Ubuntu 20.04+ ✅
 - CentOS Stream 9+ ✅
 - Rocky 9+ ✅
 - AlmaLinux 9+ ✅
 - Fedora 38+ ✅（dnf 分支覆盖）

已在 [Vultr](https://www.vultr.com/?ref=7048874) 上测试通过。

---

## 📥 一键安装 / 更新脚本

```bash
wget -O sing-box-plus.sh https://raw.githubusercontent.com/Alvin9999-newpac/Sing-Box-Plus/main/sing-box-plus.sh && chmod +x sing-box-plus.sh && bash sing-box-plus.sh
```

或者

```bash
curl -fsSL -o sing-box-plus.sh https://raw.githubusercontent.com/Alvin9999-newpac/Sing-Box-Plus/main/sing-box-plus.sh && chmod +x sing-box-plus.sh && bash sing-box-plus.sh
```

安装完成后，输入 `bash sing-box-plus.sh` 可进入管理页面。

---

## 🧭 功能菜单

```text
 🚀 Sing-Box-Plus 管理脚本 v4.8.0 🚀
 脚本更新地址: https://github.com/Alvin9999-newpac/Sing-Box-Plus
=============================================================
系统加速状态：已启用 / 未启用 BBR
Sing-Box 启动状态：运行中 / 未运行 / 未安装
=============================================================
  1) 安装/部署（20 节点）
  2) 查看分享链接（IPv4）
  6) 查看分享链接（IPv6）
  3) 重启服务
  4) 一键更换所有端口
  5) 一键开启 BBR
  8) 卸载
  0) 退出
=============================================================
```

---

## 🧩 版本更新日志

| 版本    | 日期     | 变更 |
|--------|----------|------|
| v4.8.0 | 2026-07  | 修复hysteria2的2个pinnedPeerCertSha256节点链接 |
| v4.7.0 | 2026-06  | reality协议节点增加多个域名，随机选择 |
| v4.6.0 | 2026-05  | 新增 AnyTLS 协议节点（直连 + WARP），节点总数 18 → 20 |
| v4.5.0 | 2026-05  | 固定 sing-box 版本为 v1.13.7 |

---

## 📂 文件与目录

| 路径                                             | 说明                                   |
| -------------------------------------------------- | ---------------------------------------- |
| `/usr/local/bin/sing-box`                    | sing-box 二进制                        |
| `/opt/sing-box/config.json`                  | 主配置（自动生成）                     |
| `/opt/sing-box/data/`                        | sing-box 数据目录                      |
| `/opt/sing-box/cert/{fullchain.pem,key.pem}` | 自签证书（按 `REALITY_SERVER` 生成） |
| `/opt/sing-box/ports.env`                    | 20 个端口持久化                        |
| `/opt/sing-box/env.conf`                     | 全局环境配置                           |
| `/opt/sing-box/creds.env`                    | 凭据（UUID、Reality Keypair、SS、AnyTLS 等）   |
| `/opt/sing-box/warp.env`                     | WARP 关键参数（规范化后）              |
| `/opt/sing-box/wgcf/`                        | `wgcf` 账号与 profile（历史保留）      |

---

## 🚦 使用步骤

1. **首次运行脚本** → 选择 `1) 安装/部署（20 节点）`
   * 自动安装 sing-box / jq / curl 等依赖
   * 自动生成凭据与证书、WARP 出站、写入 `config.json`
   * 自动注册 systemd 并启动
2. **查看分享链接** → `2) 查看分享链接`
   * 直连 10 与 WARP 10 **分组输出**
   * 可直接导入到 v2rayN / sing-box / Shadowrocket / Mihomo 等
3. **更换端口** → `4) 一键更换所有端口`
   * 20 个端口全部生成不冲突的新端口
   * 自动重写 `config.json` + 放行端口 + 重启服务
   * （已修复）**一次回车即可返回主菜单**
4. **开启 BBR** → `5) 一键开启 BBR`
   * 自动检测并设置 `fq + bbr`，提高拥塞控制与队列质量
5. **重启服务** → `3) 重启服务`
6. **卸载** → `8) 卸载`
   * 停止服务、移除 systemd、保留数据目录（如需全清自行删除 `/opt/sing-box`）

---

## 🔗 分享链接示例（片段）

脚本会为每个入站生成标准导入链接，例如：

```text
# 直连（示例）
vless://<UUID>@<IP>:<PORT>?encryption=none&flow=xtls-rprx-vision&security=reality&sni=www.microsoft.com&fp=chrome&pbk=<REALITY_PUB>&sid=<SID>&type=tcp#vless-reality
vmess://<Base64(JSON)>
hy2://<pwd_b64url>@<IP>:<PORT>?insecure=1&allowInsecure=1&sni=<REALITY_SERVER>#hysteria2
ss://<base64(method:password)>@<IP>:<PORT>#ss / #ss2022
tuic://<uuid>:<uuid>@<IP>:<PORT>?congestion_control=bbr&alpn=h3&insecure=1&allowInsecure=1&sni=<REALITY_SERVER>#tuic-v5
anytls://<pwd_b64url>@<IP>:<PORT>?insecure=1&sni=<REALITY_SERVER>#anytls
```

> **提示**
> 
> * VMess 采用 `ws + path=/vm`；
> * Hysteria2-OBFS：`obfs=salamander`，`alpn=h3`；
> * TUIC v5：默认 `insecure=1`，便于客户端快速导入（可自行改为严格证书校验）；
> * AnyTLS：使用标准 TLS 1.3 + Padding，需要 sing-box 1.12+ 或 Mihomo 较新版本支持。

---

## 🔧 端口放行（云防火墙）

脚本会自动尝试使用 `ufw / firewalld / iptables` 放行本机端口。若你的云提供商**额外有"安全组/云防火墙"**，请把**下方命令打印出来的端口**放行到公网：

```bash
echo "=== 必须放行到云防火墙的端口 ==="
echo "[TCP]"
jq -r '.inbounds[]|[.listen_port, (if .type|test("hysteria2|tuic") then "" else "tcp" end)]|@tsv' /opt/sing-box/config.json \
| awk -F'\t' '$2=="tcp"{print $1}' | sort -n | uniq | paste -sd',' -
echo "[UDP]"
jq -r '.inbounds[]|[.listen_port, (if .type|test("hysteria2|tuic") then "udp" else (if .type=="shadowsocks" then "both" else "" end) end)]|@tsv' /opt/sing-box/config.json \
| awk -F'\t' '$2=="udp"{print $1} $2=="both"{print $1}' | sort -n | uniq | paste -sd',' -
```

> AnyTLS 走 TCP，命令会自动包含。

---

## 🛠 常见问题（FAQ）

### 1）WARP 报错：`illegal base64 data at input byte 40`

**原因**：旧版 wgcf profile 中 `PublicKey/PrivateKey/Reserved` 含引号/回车/空格或缺失。
**脚本处理**：自动**去引号/去 CR/去空格**，Reserved 缺失回退 `0,0,0`。
**仍有旧坏值**？可一键重置：

```bash
rm -f /opt/sing-box/warp.env
rm -f /opt/sing-box/wgcf/wgcf-profile.conf   # 可选
bash sing-box-plus.sh     # 重新选择 1) 安装/部署
```

> v4.5.0+ 已默认使用 warp-cli 模式，wgcf 仅为历史兼容保留，新部署一般不会触发此问题。

### 2）更换端口后节点无法使用

* 请先确认**云防火墙**已放行新端口（见上节命令）
* 执行：

```bash
ss -lntup | grep -E 'sing-box|LISTEN'
journalctl -u sing-box.service --no-pager -n 100
```

若日志中出现 `bind: address already in use`，说明新端口与其他进程冲突 → 再次 `4) 一键更换所有端口`。

### 3）菜单"更换端口"需要按两次回车

已在 v2.1.6 内修复：现在**一次回车**即可返回主菜单。

### 4）`curl: (22) 404` 下载 sing-box 失败

* 多因 GitHub API 变更或网络不可达；脚本内已做架构/版本回退逻辑。
* v4.5.0+ 已固定 sing-box 版本为 v1.13.7，如需切换可设置环境变量：`SINGBOX_TAG=v1.13.x bash sing-box-plus.sh`
* 可稍后重试或手动上传二进制到 `/usr/local/bin/sing-box` 并赋权 `0755`。

### 5）"legacy wireguard outbound is deprecated" 的警告

* 来自 sing-box 1.12.x 的**提示**，不影响当前用法；v4.5.0+ 已迁移到 warp-cli 模式，新部署不会再出现。

### 6）AnyTLS 节点导入客户端报错或无法识别

* AnyTLS 是较新协议（sing-box v1.12 引入），需要客户端支持：
  * **sing-box** 1.12+（含 SFA / SFI / SFM 客户端 1.12+）
  * **Mihomo (Clash.Meta)** 较新版本
  * **Hiddify** 较新版本
* 老版本 **v2rayN / Shadowrocket** 不支持 AnyTLS，请更新客户端到最新版本，或手动添加节点（密码、地址、端口、SNI、insecure 这几项手填即可）。

---

## 🧹 卸载

在菜单选择 `8) 卸载`。若需**彻底清理**：

```bash
systemctl stop sing-box.service
systemctl disable sing-box.service
rm -f /etc/systemd/system/sing-box.service
systemctl daemon-reload
rm -rf /opt/sing-box
rm -f /usr/local/bin/sing-box
```

---

## ⚙️ 进阶：自定义（可选）

* `REALITY_SERVER` / `REALITY_SERVER_PORT` / `GRPC_SERVICE` / `VMESS_WS_PATH` / `ENABLE_ANYTLS` 等可在 `/opt/sing-box/env.conf` 中修改，然后：

```bash
bash sing-box-plus.sh   # 执行 3) 重启服务 或 1) 重新部署
```

* 修改证书（自签 → 正式证书）
  将你的 `fullchain.pem` / `key.pem` 放到 `/opt/sing-box/cert/` 并保持文件名一致，然后重启。

* 切换 sing-box 版本：

```bash
SINGBOX_TAG=v1.13.7 bash sing-box-plus.sh   # 或其他你想固定的版本
```

***

有问题可以[发帖](https://github.com/Alvin9999-newpac/Sing-Box-Plus/issues)反馈，或者发邮件到海外邮箱 rebeccalane27@gmail.com 进行反馈。
