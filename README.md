# 🍣 盘盘算 · Sushiro Calculator

<p align="center">
  <strong>专为寿司郎爱好者打造的高颜值、轻量级移动端消费计数器与账单生成工具</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Version-v2.5.0-crimson.svg?style=flat-square" alt="Version">
  <img src="https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square" alt="License">
  <img src="https://img.shields.io/badge/Pure_Frontend-HTML5%20%7C%20CSS3%20%7C%20JS-green.svg?style=flat-square" alt="Tech">
  <img src="https://img.shields.io/badge/Style-Liquid_Glassmorphism-indigo.svg?style=flat-square" alt="Style">
</p>

---

## 📖 项目简介

**盘盘算 (Sushiro Calculator)** 是一款纯前端单文件（Single-file）驱动的日料/寿司盘消费计算工具。告别用餐时心算金额的繁琐，随时记录各色盘子数量与非盘单点菜品，并提供充满趣味的食客画像、高清账单导出与丰富的移动端微交互。

作者：`@jory` 个人计数工具。

---

## ✨ 核心特性

- 📱 **专为移动端单手握持设计**：采用上 3 下 2 的对称网格排布，大尺寸触控区域，点击盘子即刻计数（`+1`），操作极为流畅。
- 🎨 **液态玻璃拟态（Liquid Glassmorphism）**：
  - 高级高斯磨砂玻璃（`backdrop-filter`）、微光内阴影与悬浮景深。
  - 深度适配系统级深色/浅色模式，支持右上角一键手动自由切换。
  - 温润珍珠日料风浅色渐变与沉浸式暗黑夜间主题。
- 📳 **触觉反馈引擎 (Haptic Engine 3.0)**：
  - **Android 线性马达**：精准控制起振阈值与震动周期（30ms ~ 60ms）。
  - **微信 JS-SDK / Telegram WebApp**：针对主流移动应用内嵌容器的原生底层震动桥接。
  - **Web Audio 拟态补偿**：在 iOS Safari 等限制原生震动 API 的环境下，自动激发低频沉闷正弦波（Sub-bass Click），营造逼真的机械按键触感。
- 🧾 **零依赖高清账单长图导出**：
  - 纯前端集成 `html2canvas` 引擎，一键将账单渲染为 2.5x 高分辨率 PNG 图片。
  - 优先调用移动端原生分享面板（Web Share API），可直接点击“存储图像”存至手机相册。
- 🎭 **趣味动态评价与彩蛋系统**：
  - **单品专属 Emoji 飘升**：点按各色盘子仅在点击坐标处弹出一颗诱人的对应日料食物 Emoji（玉子烧、三文鱼、甜虾、大腹等）。
  - **账单瀑布喷泉**：打开账单弹窗瞬间，从屏幕底部喷涌出 36 颗美食与欢庆 Emoji。
  - **震撼爆裂评价卡片**：结合吃碟配比（黑碟贵族、白碟大师、碳水居酒屋等）弹出毒舌食客画像。
  - **百元彩蛋（Easter Egg）**：单次消费总额达到 **¥100.00** 时，账单动画完毕后屏幕正中央将爆出超大 🐷 Emoji 与灵魂拷问弹窗。

---

## 🏷️ 官方计费规则预设

| 碟色标识 | 官方单价 | 对应经典菜式示例 |
| :---: | :---: | :--- |
| **白碟** | ¥8.00 | 玉子烧、经典寿司、小军舰 |
| **红碟** | ¥10.00 | 三文鱼、甜虾、熟章鱼 |
| **银碟** | ¥15.00 | 帆立贝、刺身寿司、天妇罗虾 |
| **黄碟** | ¥20.00 | 红楚蟹、优质单品、小食拼盘 |
| **黑碟** | ¥28.00 | 特选金枪鱼大腹、黑毛和牛、极品厚切 |

*注：支持在“单点项目”区域自定义追加非盘装餐品（拉面、茶碗蒸、炸鸡、甜品等）及对应价格。*

---

## 🚀 快速上手与运行

本项目为**纯单文件架构（Single-file Application）**，无任何前端构建工具要求（No Node.js / Webpack / Vite needed），随时随地双击即可运行。

### 1. 本地运行
直接克隆或下载仓库：
```bash
git clone https://github.com/your-username/sushiro-calculator.git
cd sushiro-calculator
```
双击 `sushiro_calculator.html`，即可在任何主流桌面或移动端浏览器中秒开使用。

### 2. 手机端便捷使用方案
- **方法一（微信/聊天传输）**：将 `.html` 文件发送到手机微信或文件传输助手，直接打开即可畅享原生震动桥接。
- **方法二（GitHub Pages 一键部署）**：
  1. 将本项目推送到 GitHub 仓库。
  2. 仓库进入 `Settings` -> `Pages`，选择 `main` 分支根目录并点击保存。
  3. 使用手机浏览器访问生成的 Pages 网址，点击分享按钮并选择**“添加到主屏幕”**，即可作为独立 Web App (PWA) 随用随开。

---

## 🛠️ 技术架构

- **核心语言**：HTML5、现代 CSS3 (Custom Properties, Flexbox, Grid, CSS Animations)、原生 ES6+ JavaScript。
- **第三方外链库**：
  - [html2canvas (v1.4.1)](https://html2canvas.hertzen.com/)：负责前端 DOM 转 Canvas 高清渲染。
- **设备 API 集成**：
  - `navigator.vibrate` (W3C Vibration API)
  - `window.AudioContext` / `webkitAudioContext` (Web Audio API)
  - `navigator.share` / `navigator.canShare` (Web Share API)

---

## 📂 项目结构

```text
.
├── sushiro_calculator.html    # 完整核心应用（单文件包含所有样式与交互逻辑）
├── CHANGELOG.md               # 历史版本演进记录
└── README.md                  # 项目使用与说明文档
```

---

## 📄 开源许可证

本项目基于 [MIT License](LICENSE) 开源，欢迎自由学习、分享与个人使用。