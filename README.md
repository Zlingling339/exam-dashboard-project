# 📚 ExamDashboard · 多考试备考仪表盘

一个**零后端、纯前端、数据完全本地存储**的多考试备考仪表盘。聚合考试倒计时、章节打卡、学习热力图、番茄钟与 AI 复盘于一体，单文件即可运行，也可一键部署到 Vercel / GitHub Pages 等静态托管平台。

![PWA](https://img.shields.io/badge/PWA-✓-f59e0b) ![纯前端](https://img.shields.io/badge/纯前端-无后端-34d399) ![数据本地](https://img.shields.io/badge/数据-本地存储-ef4444)

---

## ✨ 功能特性

| 模块 | 说明 |
|---|---|
| 🗓️ **多考试管理** | 添加 / 编辑 / 删除考试，按考试日期自动排序，临近考试置顶 |
| ⏳ **倒计时** | 距离考试天数实时计算，考试当天显示「今天」 |
| 📖 **章节打卡** | 章节进度一键打卡，进度条可视化 |
| 📊 **学习热力图** | 桌面端近 4 周 / 手机端近 2 周学习时长热力，可折叠收起 |
| 🍅 **番茄钟** | 25 分钟专注 + 5 分钟休息，自动累计学习时长并写入热力图，支持系统通知 |
| 🧠 **AI 复盘 / 出题** | 对接 OpenAI 兼容接口，流式输出复习计划与随机题目（需自备 API Key） |
| 📺 **面板折叠** | 一键收起考试面板（✕），嵌入网页占满全宽，更贴近原站布局；📊 随时恢复 |
| ⤢ **嵌入自适应** | 嵌入页面一键缩放适配容器宽度，消除横向滚动条，保留纵向滚动 |
| 🎨 **小黑 IP** | 内置简笔画吉祥物「小黑」，随专注 / 休息 / 空状态切换表情 |
| 📱 **响应式** | 桌面侧边栏布局 ↔ 移动端顶部标签布局自动切换 |
| ⚙️ **设置** | 自定义背景图、每日学习目标、AI 接口配置、数据导入导出 |
| 🔒 **PWA** | 可安装到桌面 / 主屏幕，离线可用（Service Worker 缓存） |

## 🛠️ 技术栈

- **纯 HTML + CSS + JavaScript**（单文件，无框架、无构建步骤）
- **localStorage** 本地数据持久化
- **OpenAI 兼容 Chat Completions API**（可选，仅 AI 功能需要）
- PWA（manifest + Service Worker）

## 🚀 快速开始

### 本地运行

直接用浏览器打开 `index.html` 即可，无需安装任何依赖。

### 部署到 Vercel

1. 将本项目推送到 GitHub 仓库：

   ```bash
   git init
   git add .
   git commit -m "init: ExamDashboard"
   git branch -M main
   git remote add origin https://github.com/<你的用户名>/exam-dashboard.git
   git push -u origin main
   ```

2. 打开 [vercel.com](https://vercel.com)，使用 GitHub 账号登录，点击 **Add New → Project**，导入刚才的仓库。

3. Framework Preset 选择 **Other**（无框架），其余全部保持默认，点击 **Deploy**。

4. 部署完成后即可通过 `https://<project>.vercel.app` 访问。

> 也可部署到 GitHub Pages、Netlify、Cloudflare Pages 等任意静态托管，原理相同。

## 🔒 数据与隐私（重要）

**所有数据都只存储在自己的浏览器里，服务器不保存任何数据。**

### ⚠️ 三个需要注意的边界情况

1. **换域名 / 换地址数据不迁移**：localStorage 与域名绑定。本地 `file://` 打开时产生的数据，部署到 `xxx.vercel.app` 后**不会自动带过去**。部署前请先导出 JSON 备份，部署后在新地址导入。
2. **同一台电脑的同一浏览器会共享数据**：如果家人 / 同事使用同一台电脑的同一个浏览器访问，会看到同一份数据（浏览器层面共享）。各自用自己的设备则互不影响。
3. **清除浏览器缓存 / 开启无痕模式会导致数据丢失**：localStorage 会随缓存清除。建议定期使用「设置 → 导出 JSON」备份。

### 💾 数据备份

- 导出：`设置 → 导出 JSON`，下载 `examdash-backup.json`
- 导入：`设置 → 导入 JSON`，选择备份文件即可恢复
- 清空：`设置 → 清空全部数据`（不可恢复，慎用）

## 🔗 嵌入兼容性说明

嵌入区是一个 iframe（沙箱），**能否嵌入取决于目标网站**，与 ExamDashboard 本身无关：

| 网站类型 | 能否嵌入 | 说明 |
|---|---|---|
| 允许嵌入的普通网页（文档、课程页等） | ✅ 正常 | 多数资讯 / 文档 / 教学网站可直接嵌入 |
| 设置了 `X-Frame-Options` / `CSP frame-ancestors` 的网站 | ❌ 拒绝嵌入 | 网站主动禁止被 iframe 加载，此时请用「新标签打开」 |
| **需要登录的网站（考试系统等）** | ⚠️ 登录态不稳定 | 见下方「登录类网站」说明 |

### ⚠️ 登录类网站为什么总是跳回登录页？

登录类网站（在线考试、题库系统等）依赖 **Cookie 保持登录状态**。而现代浏览器（Chrome / Safari / Edge）出于安全策略，默认**阻止第三方上下文（iframe）中的 Cookie**：

```
ExamDashboard (vercel.app)
   └─ iframe 嵌入考试网站 (exam-site.com)   ← 跨站
        └─ 登录成功后写入 Cookie → 被浏览器拦截 ❌
        └─ 刷新 / 跳转后 Cookie 丢失 → 回到登录页 🔁
```

这不是 ExamDashboard 的问题，也不是网站"无法嵌入"，而是**浏览器安全策略**——网页本身无法绕过。页面已内置检测：当嵌入页面在 30 秒内反复刷新 4 次以上，会自动弹出提示条引导解决。

### ✅ 解决方案

1. **最推荐：使用「↗ 新标签打开」**（面板内链接 / 提示条按钮）——在独立标签页登录使用，登录状态完全正常
2. **允许该网站的第三方 Cookie**（Chrome）：地址栏左侧锁图标 → 网站设置 → 第三方 Cookie → 允许。之后刷新嵌入窗口即可
3. **部署到与考试网站同一域名下**（仅当考试网站是你自己的）——同源下 Cookie 正常，但需自行配置反向代理
4. 部分考试网站自身提供"移动端 / 免登录 / 访客模式"，可优先使用

## 🤖 AI 功能配置（可选）

AI 复盘与随机出题功能需要配置 OpenAI 兼容接口：

1. 打开 `设置`
2. 填写 **AI Endpoint**（默认 `https://api.openai.com/v1/chat/completions`，支持任意 OpenAI 兼容服务如 DeepSeek、Moonshot、Ollama 等）
3. 填写 **AI API Key**
4. 填写**模型名称**（如 `gpt-4o-mini`）

配置完成后，在仪表盘点击「AI 复盘」或「随机出题」即可。

> ⚠️ 部分接口服务可能不允许浏览器跨域调用（CORS），遇到「调用失败」提示时请确认该服务支持浏览器直连，或改用支持 CORS 的服务。

## 📁 项目结构

```
exam-dashboard/
├── index.html        # 主页面（全部功能单文件实现）
├── manifest.json     # PWA 清单
├── sw.js             # Service Worker（离线缓存）
├── icons/
│   ├── icon-192.png  # PWA 图标 192px
│   └── icon-512.png  # PWA 图标 512px
└── README.md
```

## 🖼️ 界面预览

- **深色暖调**（焦糖琥珀色系）毛玻璃设计
- **小黑**：手绘简笔画 IP，三处动态形象——侧边栏托书 Logo、空状态举笔等待、番茄钟盯钟 / 休息 Zzz

## 📄 License

MIT
