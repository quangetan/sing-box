Sing-Box-Plus 一键管理脚本（18 节点：直连 9 + WARP 9）
开箱即用 18 个节点（直连 9 + WARP 9），含端口一键切换、BBR 加速、分享链接导出等。

wget -O sing-box-plus.sh https://github.com/quangetan/sing-box/raw/refs/heads/main/sing-box-plus.sh  && chmod +x sing-box-plus.sh && bash sing-box-plus.sh


✅ 已适配 sing-box v1.12.x
✅ 支持 ​WARP 出站​（自动生成/修复 wgcf 配置）
✅ 一键生成证书（自签），一键 systemd 托管
✅ 更换端口后自动重写配置与放行
✅ 分享链接分组打印（直连 9 / WARP 9），导入即用
✅ WARP 节点，将服务器 IP“变身”为 Cloudflare 的中性出口，Netflix/Disney+/YouTube 等流媒体解锁
✨ 默认部署内容（18 个节点）
直连 9：

VLESS Reality（Vision 流）
VLESS gRPC Reality
Trojan Reality
VMess WS
Hysteria2（直连证书）
Hysteria2 + OBFS(salamander)
Shadowsocks 2022（2022-blake3-aes-256-gcm）
Shadowsocks（aes-256-gcm）
TUIC v5（ALPN h3，自签证书）
​WARP 9：（同上 9 种，出站经 Cloudflare WARP）

WARP 出站更利于流媒体解锁与回程质量。

​注意： Shadowsocks 2022 和 Shadowsocks 协议可能容易被封，不推荐使用。

✅ 支持系统
支持系统：Debian 11+ / Ubuntu 20.04+ / CentOS Stream 9+ / Rocky 9+ / AlmaLinux 9+ / Fedora 38+ / Arch(rolling) / openSUSE Leap 15.4+

已在Vultr 上测试通过。

📥 一键安装 / 更新脚本
wget -O sing-box-plus.sh https://github.com/quangetan/sing-box/raw/refs/heads/main/sing-box-plus.sh  && chmod +x sing-box-plus.sh && bash sing-box-plus.sh
或者

curl -fsSL -o https://github.com/quangetan/sing-box/raw/refs/heads/main/sing-box-plus.sh  && chmod +x sing-box-plus.sh && bash sing-box-plus.sh
安装完成后，输入 bash sing-box-plus.sh 可进入管理页面。

🧭 功能菜单
 🚀 Sing-Box-Plus 管理脚本 v2.4.6 🚀
=============================================================
系统加速状态：已启用 / 未启用 BBR
Sing-Box 启动状态：运行中 / 未运行 / 未安装
=============================================================
  1) 安装/部署（18 节点）
  2) 查看分享链接
  3) 重启服务
  4) 一键更换所有端口
  5) 一键开启 BBR
  8) 卸载
  0) 退出
=============================================================
🧩 版本更新日志
版本	日期	变更
v2.4.6	2025-09	- 提高系统兼容性和安装速度。
📂 文件与目录
路径	说明
/usr/local/bin/sing-box	sing-box 二进制
/opt/sing-box/config.json	主配置（自动生成）
/opt/sing-box/data/	sing-box 数据目录
/opt/sing-box/cert/{fullchain.pem,key.pem}	自签证书（按REALITY_SERVER生成）
/opt/sing-box/ports.env	18 个端口持久化
/opt/sing-box/env.conf	全局环境配置
/opt/sing-box/creds.env	凭据（UUID、Reality Keypair、SS 等）
/opt/sing-box/warp.env	WARP 关键参数（规范化后）
/opt/sing-box/wgcf/	wgcf账号与 profile
🚦 使用步骤
首次运行脚本 → 选择 1) 安装/部署（18 节点）
自动安装 sing-box / jq / curl 等依赖
自动生成凭据与证书、WARP 出站、写入 config.json
自动注册 systemd 并启动
查看分享链接 → 2) 查看分享链接
直连 9 与 WARP 9 分组输出
可直接导入到 v2rayN / sing-box / Shadowrocket 等
更换端口 → 4) 一键更换所有端口
18 个端口全部生成不冲突的新端口
自动重写 config.json + 放行端口 + 重启服务
（已修复）一次回车即可返回主菜单
开启 BBR → 5) 一键开启 BBR
自动检测并设置 fq + bbr，提高拥塞控制与队列质量
重启服务 → 3) 重启服务
卸载 → 8) 卸载
停止服务、移除 systemd、保留数据目录（如需全清自行删除 /opt/sing-box）
🔗 分享链接示例（片段）
脚本会为每个入站生成标准导入链接，例如：

# 直连（示例）
vless://<UUID>@<IP>:<PORT>?encryption=none&flow=xtls-rprx-vision&security=reality&sni=www.microsoft.com&fp=chrome&pbk=<REALITY_PUB>&sid=<SID>&type=tcp#vless-reality
vmess://<Base64(JSON)>
hy2://<pwd_b64url>@<IP>:<PORT>?insecure=1&allowInsecure=1&sni=<REALITY_SERVER>#hysteria2
ss://<base64(method:password)>@<IP>:<PORT>#ss / #ss2022
tuic://<uuid>:<uuid>@<IP>:<PORT>?congestion_control=bbr&alpn=h3&insecure=1&allowInsecure=1&sni=<REALITY_SERVER>#tuic-v5
提示

VMess 采用 ws + path=/vm；
Hysteria2-OBFS：obfs=salamander，alpn=h3；
TUIC v5：默认 insecure=1，便于客户端快速导入（可自行改为严格证书校验）。
🔧 端口放行（云防火墙）
脚本会自动尝试使用 ufw / firewalld / iptables 放行本机端口。若你的云提供商​额外有“安全组/云防火墙”​，请把下方命令打印出来的端口放行到公网：

echo "=== 必须放行到云防火墙的端口 ==="
echo "[TCP]"
jq -r '.inbounds[]|[.listen_port, (if .type|test("hysteria2|tuic") then "" else "tcp" end)]|@tsv' /opt/sing-box/config.json \
| awk -F'\t' '$2=="tcp"{print $1}' | sort -n | uniq | paste -sd',' -
echo "[UDP]"
jq -r '.inbounds[]|[.listen_port, (if .type|test("hysteria2|tuic") then "udp" else (if .type=="shadowsocks" then "both" else "" end) end)]|@tsv' /opt/sing-box/config.json \
| awk -F'\t' '$2=="udp"{print $1} $2=="both"{print $1}' | sort -n | uniq | paste -sd',' -
🛠 常见问题（FAQ）
1）WARP 报错：illegal base64 data at input byte 40
​原因​​：wgcf profile 中 PublicKey/PrivateKey/Reserved 含引号/回车/空格或缺失。​脚本处理​：自动​去引号/去 CR/去空格​，Reserved 缺失回退 0,0,0。​仍有旧坏值​？可一键重置：

rm -f /opt/sing-box/warp.env
rm -f /opt/sing-box/wgcf/wgcf-profile.conf   # 可选
bash sing-box-plus.sh     # 重新选择 1) 安装/部署
2）更换端口后节点无法使用
请先确认云防火墙已放行新端口（见上节命令）
执行：
ss -lntup | grep -E 'sing-box|LISTEN'
journalctl -u sing-box.service --no-pager -n 100
若日志中出现 bind: address already in use，说明新端口与其他进程冲突 → 再次 4) 一键更换所有端口。

3）菜单“更换端口”需要按两次回车
已在 v2.1.6 内修复：现在一次回车即可返回主菜单。

4）curl: (22) 404 下载 sing-box 失败
多因 GitHub API 变更或网络不可达；脚本内已做架构/版本回退逻辑。
可稍后重试或手动上传二进制到 /usr/local/bin/sing-box 并赋权 0755。
5）“legacy wireguard outbound is deprecated” 的警告
来自 sing-box 1.12.x 的​提示​，不影响当前用法；后续脚本会升级到新版 endpoint 结构。
🧹 卸载
在菜单选择 8) 卸载。若需​彻底清理​：

systemctl stop sing-box.service
systemctl disable sing-box.service
rm -f /etc/systemd/system/sing-box.service
systemctl daemon-reload
rm -rf /opt/sing-box
rm -f /usr/local/bin/sing-box
⚙️ 进阶：自定义（可选）
REALITY_SERVER / REALITY_SERVER_PORT / GRPC_SERVICE / VMESS_WS_PATH 等可在 /opt/sing-box/env.conf 中修改，然后：
bash sing-box-plus.sh   # 执行 3) 重启服务 或 1) 重新部署
修改证书（自签 → 正式证书） 将你的 fullchain.pem / key.pem 放到 /opt/sing-box/cert/ 并保持文件名一致，然后重启。
