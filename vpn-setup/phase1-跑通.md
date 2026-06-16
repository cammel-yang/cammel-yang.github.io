# P1 · 跑通：一台 VPS + Reality 节点

目标：**最小可用**。一台海外 VPS，部署一个 VLESS+Vision+Reality 节点，
让你自己的一台设备（手机或电脑）能稳定翻墙。先验证链路，别管多设备和备份。

预计耗时：30–60 分钟。

---

## 第 1 步：租一台 VPS

推荐先用 **Vultr**（按小时计费，IP 被封可随时销毁重建，适合起步）：

1. 注册 https://www.vultr.com/ ，绑定支付方式。
2. Deploy New Server → Cloud Compute。
3. **机房**：Tokyo（东京）/ Singapore，离国内近、延迟低。
4. **系统**：Debian 12 x64。
5. **规格**：最低配（1 vCPU / 1GB）即可，约 $5–6/月。
6. 部署完成后记下：**服务器 IP**、**root 密码**。

> 其它可选厂商：DigitalOcean、AWS Lightsail、搬瓦工（CN2 GIA 线路更快但更贵）。
> 步骤大同小异，把"机房/系统/IP"对应上即可。

## 第 2 步：登录服务器并加固（基础）

本机终端执行（把 `SERVER_IP` 换成你的 IP）：

```bash
ssh root@SERVER_IP
```

进去后先更新系统：

```bash
apt update && apt upgrade -y
```

> SSH 密钥登录、改端口等更完整的加固见 README「安全底线」。P1 先跑通，加固可稍后补。

## 第 3 步：一键安装 Xray（Reality）

社区维护的 233boy 脚本，菜单式操作，自动生成节点链接：

```bash
bash <(curl -L https://github.com/233boy/Xray/raw/main/install.sh)
```

安装时：

1. 协议选 **VLESS**（或直接选带 `Reality` 的项）。
2. 回车使用默认端口 / 默认偷取的域名（脚本会自动选一个常见大站如 `www.microsoft.com` 做伪装）。
3. 装完脚本会直接打印一条 **`vless://...` 节点链接**和二维码 —— 复制保存好。

> 想手动理解配置结构，可参考本目录 `configs/xray-reality-server.json` 模板
> （真实的 UUID、私钥由脚本自动生成，不要手填）。

常用管理命令：

```bash
v2ray          # 打开管理菜单（查看/修改/重启）
v2ray info     # 再次查看节点链接和二维码
```

## 第 4 步：本地客户端导入

挑一个客户端，把上面的 `vless://...` 链接导入：

| 平台 | 客户端 | 导入方式 |
|------|--------|----------|
| Windows | v2rayN / Nekoray / Clash Verge | 从剪贴板导入 |
| macOS | Clash Verge / ClashX Meta | 从剪贴板导入 |
| Android | v2rayNG / NekoBox | 扫码或粘贴链接 |
| iOS | Shadowrocket（小火箭，需外区 App Store） | 扫码或粘贴 |

导入后：选中该节点 → 开启代理（系统代理 / Tun 模式）。

## 第 5 步：验证

1. 浏览器访问一个被墙的网站（如 google.com）能打开。
2. 搜索"my ip"，确认显示的是你 VPS 的 IP / 所在国家。
3. 在客户端里看延迟（ping）：东京线路一般几十到一百多毫秒算正常。

✅ 都通过 = P1 完成，你已经有了一个完全自己掌控的节点。

---

## 常见问题

- **连不上 / 一直转圈**：
  - 确认客户端开了"系统代理"或"Tun 模式"。
  - 确认 VPS 防火墙放行了节点端口（Vultr 默认全放，问题不大）。
  - 换个端口或重新生成节点（`v2ray` 菜单里操作）。
- **能连但很慢**：换机房（东京/新加坡/美西多试），或升级带宽套餐。
- **用一阵突然全挂**：可能 IP 被封 → Vultr 上销毁实例、重建一台新 IP，重跑第 3 步。这也是 P3 要加备用节点的原因。

## 稳定几天后 → 进入 [P2 多设备](./phase2-多设备.md)
