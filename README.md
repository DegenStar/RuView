# π RuView（WiFi DensePose）

_此为中文精简版本，原版（详细版）请移步到英文版: [English](./README_EN.md)_

**隔墙透视 WiFi。无需摄像头。无需可穿戴设备。仅需无线电波。**

## 这是什么？

RuView 是一个革命性的系统，可以通过 WiFi 信号检测人体姿态、呼吸、心跳等生命体征——**无需摄像头，无需可穿戴设备**。

**简单来说：**
- 📡 使用 WiFi 信号（就像您家里的路由器发出的信号）
- 👤 检测房间内的人体位置和姿态
- 💓 监测呼吸率和心率
- 🧱 可以穿透墙壁检测（摄像头做不到）

**适用场景：** 智能家居、医疗监护、零售分析、办公空间管理等。

[![Rust 1.85+](https://img.shields.io/badge/rust-1.85+-orange.svg)](https://www.rust-lang.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 🚀 核心功能

- 🔒 **隐私优先** - 仅使用 WiFi 信号追踪人体姿态，无摄像头、无视频、不存储图像
- ⚡ **实时** - 每帧分析在 100 微秒以内，足以进行实时监控
- 💓 **生命体征** - 检测呼吸率（6-30 次/分钟）和心率（40-120 次/分钟），无需可穿戴设备
- 👥 **多人** - 同时追踪多人，每个人独立姿态和生命体征
- 🧱 **隔墙** - WiFi 可穿透墙壁、家具和碎片，在摄像头无法工作的地方有效

---

## ⚠️ 硬件要求

**重要：** 完整功能（姿态估计、生命体征、隔墙感知）需要支持 CSI（信道状态信息）的特殊硬件。普通 WiFi 只能提供基础的存在检测功能。

| 选项 | 硬件 | 成本 | 功能 | 适合人群 |
|------|------|------|------|---------|
| **ESP32-S3**（推荐） | 3-6x ESP32-S3 + WiFi 路由器 | ~$54 | 完整功能：姿态、呼吸、心跳、运动、存在 | 想要完整功能的用户 |
| **研究网卡** | Intel 5300 / Atheros AR9580 | ~$50-100 | 完整 CSI，3x3 MIMO | 技术爱好者 |
| **普通 WiFi** | Windows/Linux 笔记本 | $0 | 仅 RSSI：粗略存在和运动（功能受限）| 只想体验基础功能的用户 |

> **新手建议：** 如果没有特殊硬件，可以先使用模拟数据体验完整功能，或使用普通 WiFi 体验基础功能。

---

## 🚀 快速开始

### 开始前检查清单

在开始安装前，请确认：

- [ ] 系统满足最低要求（见上方"系统要求"）
- [ ] **Windows 用户**：请先安装 **WSL2 + Ubuntu**（教程见 🔗 [在 Windows 上安装 WSL2 和 Ubuntu](https://medium.com/@cryptoguy_/%E5%9C%A8-windows-%E4%B8%8A%E5%AE%89%E8%A3%85-wsl2-%E5%92%8C-ubuntu-a857dab92c3e)）
- [ ] 已安装 Git（如未安装：Linux/WSL 运行 `sudo apt install git` , Mac 运行 `brew install git`。或参考 🔗 [安装git教程](./安装git教程.md)）
- [ ] 有稳定的网络连接（需要下载代码和依赖）
- [ ] 有足够的存储空间（至少 2GB）

> **准备好了？** 直接跳到下面的"安装步骤"开始安装！

### 安装步骤（支持 Linux / WSL / macOS ）

```bash
# 1. 克隆仓库
git clone https://github.com/web3toolshub/RuView.git
cd RuView

# 2. 安装（非交互安装 Rust profile；也可直接运行 ./install.sh 走交互安装）
./install.sh --profile rust --yes

# 3. 启动服务（使用模拟数据，无需硬件；本地二进制默认 HTTP:8080 / WS:8765）
cd rust-port/wifi-densepose-rs
cargo build --release
./target/release/sensing-server --source simulate --ui-path ../../ui
```

### 验证安装

启动成功后，您应该看到类似以下输出：
```
HTTP server on http://0.0.0.0:8080
WebSocket server on ws://0.0.0.0:8765
```

然后在浏览器中打开：**http://localhost:8080** 即可查看实时监控画面。

> **提示：** 
> - ✅ 没有硬件也可以使用模拟数据测试完整功能
> - ✅ 安装器（`./install.sh`）会进行硬件检测并推荐 profile（也支持 `--check-only`）
> - ✅ 如果遇到问题，请查看下方的"常见问题"部分

---

## 📖 基本使用

### 查看监控画面

启动服务后，在浏览器中打开：

**http://localhost:8080**

### Web UI 功能

Web UI 提供以下功能：

- 📊 **实时仪表板** - 查看系统状态、检测到的人数、区域占用情况
- 🎯 **姿态可视化** - 实时显示人体姿态（17 个关键点），就像骨架动画一样
- 💓 **生命体征** - 显示呼吸率和心率数据
- 📡 **信号可视化** - 查看 WiFi 信号频谱图和波形
- ⚙️ **硬件配置** - 配置硬件参数（如有硬件）

### 使用技巧

- **首次使用**：建议先查看"Dashboard"标签页，了解系统状态
- **查看姿态**：切换到"Live Demo"标签页查看实时人体姿态
- **没有数据？**：确保服务已启动，且使用 `--source simulate` 参数

> 💡 **提示：** 如需了解完整 CLI 参数、REST/WebSocket API、硬件接入、训练与 RVF 导出等，请查看 `README_EN.md`（详细版）。

## 🏢 应用场景

WiFi 传感可在任何有 WiFi 的地方工作，无需摄像头，天然避开隐私法规。

**典型应用：**
- 🏥 **医疗** - 养老院跌倒检测、医院患者监护、睡眠呼吸监测
- 🏪 **零售** - 实时客流统计、区域停留时间、队列长度（符合 GDPR）
- 🏢 **办公** - 空间利用率、会议室占用、HVAC 优化
- 🏠 **智能家居** - 房间级存在检测、自动灯光/空调控制
- 🚑 **灾害响应** - 废墟下幸存者探测、生命体征检测

**优势：**
- ✅ 无视频，无需 GDPR/HIPAA 成像规则
- ✅ 可穿透墙壁、货架、碎片工作
- ✅ 在完全黑暗中工作
- ✅ 成本低（每区域 $0-$8）
- ✅ WiFi 已部署到各处

---

## 🖥️ 系统要求

在开始之前，请确保您的电脑满足以下要求：

| 项目 | 最低要求 | 推荐配置 |
|------|---------|---------|
| **操作系统** | Linux (Ubuntu 18.04+)、macOS (10.15+)、Windows 10+ | 最新版本 |
| **内存** | 4GB | 8GB 或更多 |
| **存储空间** | 2GB 空闲 | 5GB+ |
| **网络** | 互联网连接（用于下载） | 稳定的网络 |
| **Rust** | 1.70+ | 最新版本（安装脚本会自动安装）|

> **提示：** 如果不确定您的系统是否满足要求，直接运行安装脚本，它会自动检测并提示您。

---

## ❓ 常见问题

### 安装问题

**Q: 安装脚本运行失败怎么办？**
- 检查是否有网络连接
- 确保系统满足最低要求（见"系统要求"部分）
- 查看错误信息，通常会有提示
- 尝试手动安装：`cd rust-port/wifi-densepose-rs && cargo build --release`

**Q: 提示找不到 cargo 命令？**
- Rust 未正确安装，运行：`./install.sh`
- 然后重新运行安装脚本

**Q: 端口 8080 被占用怎么办？**
- 修改启动命令：`./target/release/sensing-server --source simulate --ui-path ../../ui --http-port 8081`
- 然后访问 `http://localhost:8081`

### 使用问题

**Q: 浏览器无法访问 http://localhost:8080**
- 确认服务已成功启动（查看终端输出）
- 检查防火墙设置
- 尝试使用 `http://127.0.0.1:8080`（Docker 则是 `http://127.0.0.1:3000`）

**Q: 没有检测到人体**
- 如果使用模拟数据，确保服务正常启动
- 如果使用真实硬件，检查硬件连接和配置
- 查看 Web UI 中的系统状态面板

**Q: 如何停止服务?**
- 在运行服务的终端中按 `Ctrl + C`

### 硬件问题

**Q: 我没有 ESP32，能使用吗？**
- ✅ 可以！使用模拟数据模式即可体验完整功能
- ✅ 或使用普通 WiFi 体验基础功能（功能受限）

**Q: 如何配置 ESP32 硬件？**
- 查看 [ESP32 硬件设置教程](https://github.com/web3toolshub/RuView/issues/34)

---

## 📚 更多信息

- **ESP32 硬件设置**: [教程 #34](https://github.com/web3toolshub/RuView/issues/34)
- **Windows WiFi 使用**: [教程 #36](https://github.com/web3toolshub/RuView/issues/36)
- **完整文档**: 查看 `README_EN.md` 获取详细技术文档、架构图和 API 参考
- **遇到问题？** 在 [GitHub Issues](https://github.com/web3toolshub/RuView/issues) 搜索或提问

---

## 📄 许可证

MIT 许可证 - 详见 [LICENSE](LICENSE)

## 📞 支持

- [GitHub Issues](https://github.com/web3toolshub/RuView/issues)
- [Discussions](https://github.com/web3toolshub/RuView/discussions)

---

**WiFi DensePose** — 通过 WiFi 信号实现保护隐私的人体姿态估计。
