# OpenWrt-K

[![GitHub Repo stars](https://img.shields.io/github/stars/rheatin/OpenWrt-K)](https://github.com/rheatin/OpenWrt-K/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/rheatin/OpenWrt-K)](https://github.com/rheatin/OpenWrt-K/forks)
[![Workflow Status](https://github.com/rheatin/OpenWrt-K/actions/workflows/build-openwrt.yml/badge.svg)](https://github.com/rheatin/OpenWrt-K/actions)
> OpenWrt 软件包与定制固件自动化云编译流水线

---

## 目录

1. [更新日志](#更新日志)
2. [固件特色与定位](#固件特色与定位)
3. [内置功能与插件](#内置功能与插件)
4. [鸣谢与参考项目](#鸣谢与参考项目)

---

## 更新日志

[2026/09] 升级至 OpenWrt v25.12.5，重构并精简插件架构，实现配置全量固化与极速开箱体验
<details><summary>更新详情</summary>

1. **架构与系统升级**：
   - 升级基础系统到 **OpenWrt v25.12.5**（Linux 内核 6.12，支持 APK 包管理与 eBPF）。
   - 支持 `kmod-sched-bpf` 等新特性。
2. **核心插件重构与精简**：
   - **新增/保留**：`OpenClash` (Fake-IP / Smart 模式)、`Aurora 主题及配置`、`Argon 主题`、`Bandix` (实时带宽监控)、`QoS-Mate & LuCI` (智能流控调度)、`Lucky 大吉` (反向代理/DDNS/STUN)、`Watchdog 看门狗`、`UPnP`、`VLMCSD KMS`、`CIFS Mount`、`WOL` 等。
   - **移除冗余**：彻底移除 `SmartDNS`、`AdGuardHome`、`Turbo ACC`、`Netdata`、`Passwall`、`DiskMan` 等历史包及 35 万行旧规则，极大缩减固件体积并提升系统纯净度。
3. **CI/CD 流水线与配置固化**：
   - 固化网络参数（PPPoE 拨号、LAN 静态 IP、SSH 自定义端口 `8622`、Webex 会议直连 Bypass 规则等），实现刷机后即用。
   - 优化 CI 动态配置拉取逻辑，加入 Gzip 与 SCP 传输流双重压缩，大幅节省传输时间与带宽。

</details>

<details><summary>历史更新日志</summary>

- [2024/09/26] 使用 Python 重构了编译辅助工作流 (`build_helper`)，提升可维护性并大幅优化缓存命中率。
- [2023/07/27] 添加多配置编译支持，解耦各模块配置。

</details>

---

## 固件特色与定位

1. **官方源码底座**：基于 OpenWrt 官方最新源码编译，稳定性与安全性有保障。
2. **开箱即用，零配置烦恼**：预置精调网络参数、DNS 劫持转发、主题样式与反向代理规则，刷机后即可无缝接管家庭网络。
3. **智能流量与队列调度**：集成现代化的 `QoS-Mate` 队列算法与 `Bandix` 网卡级流量实时监控。
4. **科学代理与分流集成**：内置 `OpenClash`（Meta / Mihomo 内核），预置国内外规则与 Fake-IP DNS 优化，兼顾高速与低延迟。
5. **全量 kmod 驱动支持**：随 Release 同步打包全量内核模块（`allkmod.zip`），彻底解决 OpenWrt 官方内核 Hash 变动后无法离线安装驱动的痛点。
6. **现代化外观体验**：默认启用深度定制的 **Aurora** 主题（支持自定义毛玻璃、色彩微调与快捷工具栏），兼备经典 **Argon** 主题。

---

## 内置功能与插件

### 1. LuCI 插件 & 核心服务

| 插件名称 | 说明 | 仓库 / 来源 |
| :--- | :--- | :--- |
| **luci-app-openclash** | 强大的规则分流客户端（Clash Meta / Mihomo） | [OpenClash](https://github.com/vernesong/OpenClash) |
| **luci-app-qosmate** & **qosmate** | 现代化智能 QoS 队列管理与拥塞控制 | [qosmate](https://github.com/hudra0/qosmate) |
| **luci-app-bandix** & **bandix** | 网卡级实时带宽监视器与设备流量统计 | [openwrt-bandix](https://github.com/timsaya/openwrt-bandix) |
| **luci-app-lucky** & **lucky** | 软路由神器（端口转发 / 反向代理 / DDNS / WOL / STUN） | [lucky](https://github.com/gdy666/luci-app-lucky) |
| **luci-app-aurora-config** | 现代化 Aurora 主题个性化配置 | [luci-app-aurora-config](https://github.com/eamonxg/luci-app-aurora-config) |
| **luci-app-argon-config** | 经典 Argon 主题设置 | [luci-app-argon-config](https://github.com/jerrykuku/luci-app-argon-config) |
| **luci-app-watchdog** | 网络连接与服务自动监控/看门狗 | [luci-app-watchdog](https://github.com/sirpdboy/luci-app-watchdog) |
| **luci-app-upnp** | 通用即插即用（UPnP / miniupnpd-nftables） | 官方源 |
| **luci-app-vlmcsd** & **vlmcsd** | 局域网 KMS 自动激活服务 | [luci-app-vlmcsd](https://github.com/immortalwrt/luci) |
| **luci-app-cifs-mount** | SMB / CIFS 远程网络共享挂载工具 | 官方/第三方拓展 |
| **luci-app-wol** | 局域网设备网络唤醒（Wake-on-LAN） | 官方源 |
| **luci-app-firewall** | 基于 nftables (fw4) 的防火墙 Web 管理 | 官方源 |
| **luci-app-package-manager** | APK 软件包图形化管理界面 | 官方源 |

### 2. 主题

- **Aurora**：`luci-theme-aurora`（默认激活，已预置琥珀金样式与毛玻璃背景）
- **Argon**：`luci-theme-argon`
- **Bootstrap**：`luci-theme-bootstrap`

### 3. 实用系统与网络工具

- **网络与测速**：`iperf3-ssl`、`netperf`、`ethtool-full`、`iputils-arping`、`ip-full`、`tc-full`
- **磁盘与文件系统**：`fdisk`、`cfdisk`、`parted`、`lsblk`、`btrfs-progs`、`dosfstools`、`exfat-fsck/mkfs`、`ntfs-3g`、`smartmontools`
- **终端与管理**：`ttyd`、`htop`、`sudo`、`bash`、`coremark`、`bc`、`jq`、`unzip`、`openssh-sftp-server`、`vsftpd-tls`、`pciutils`、`usbutils`

### 4. 常见网卡驱动支持

覆盖主流千兆/2.5G/万兆网卡及虚拟化网卡：
- **Intel**：`e1000`, `e1000e`, `igb`, `igbvf`, `igc` (2.5G), `ixgbe` (万兆), `i40e`, `iavf`
- **Realtek**：`r8168`, `r8169`, `r8125` (2.5G), `r8152` (USB)
- **Mellanox / Broadcom / QEMU**：`mlx4-core`, `mlx5-core`, `bnx2`, `bnx2x`, `virtio_net`, `vmxnet3` 等

---

## 鸣谢与参考项目

感谢以下开源项目与各位维护者的无私付出：

- [openwrt/openwrt](https://github.com/openwrt/openwrt/)
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
- [immortalwrt/immortalwrt](https://github.com/immortalwrt/immortalwrt/)
- [vernesong/OpenClash](https://github.com/vernesong/OpenClash)
- [eamonxg/luci-theme-aurora](https://github.com/eamonxg/luci-theme-aurora)
- [hudra0/qosmate](https://github.com/hudra0/qosmate) & [luci-app-qosmate](https://github.com/hudra0/luci-app-qosmate)
- [timsaya/openwrt-bandix](https://github.com/timsaya/openwrt-bandix)
- [gdy666/luci-app-lucky](https://github.com/gdy666/luci-app-lucky)
- [sirpdboy/luci-app-watchdog](https://github.com/sirpdboy/luci-app-watchdog)
- [jerrykuku/luci-theme-argon](https://github.com/jerrykuku/luci-theme-argon)
