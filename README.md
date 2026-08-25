<div align="center">

# 🧹 c-disk-cleaner

**让 AI 帮你安全清理 Windows C 盘**

**Let AI safely clean up your Windows C drive**

[![Version](https://img.shields.io/badge/version-1.2.0-blue.svg)](https://github.com/Zhu0512-ops/c-disk-cleaner/releases)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Windows](https://img.shields.io/badge/Windows-10/11-0078D6?logo=windows)](https://www.microsoft.com/windows)
[![PowerShell](https://img.shields.io/badge/PowerShell-5.0+-5391FE?logo=powershell)](https://learn.microsoft.com/powershell/)
[![AI Agent](https://img.shields.io/badge/AI%20Agent-Universal-orange.svg)](#compatible-with--)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

---

**[English](#-) | [中文](#-1)**

---

</div>

## 🤔 What is this? / 这是什么？

**c-disk-cleaner** is a **universal AI agent skill** that turns an LLM into a careful Windows C-drive storage assistant. It scans candidate files, assigns risk levels, shows a precise cleanup plan, and performs only the low-risk, explicitly confirmed actions.

**c-disk-cleaner** 是一个**通用 AI Agent 技能**，将大模型变成谨慎的 Windows C 盘空间助手。它扫描候选文件、标注风险等级、展示精确清理计划，并且只执行低风险且经明确确认的操作。

> 💡 **Why AI? / 为什么要用 AI?**
>
> Traditional cleanup tools either delete too aggressively (risky) or too conservatively (waste space). An AI agent understands context, explains what it found, and follows your preferences.
>
> 传统清理工具要么删太猛（危险），要么太保守（释放空间少）。AI Agent 懂上下文、能解释找到了什么、还能按你的偏好来。

---

## ✨ Features / 功能亮点

<table>
<tr>
<td align="center" width="20%">🔍<br><b>14+ Categories</b><br>全面扫描</td>
<td align="center" width="20%">🛡️<br><b>4-Level Safety</b><br>四级安全分级</td>
<td align="center" width="20%">⚡<br><b>Confirmed Clean</b><br>确认后快速清理</td>
<td align="center" width="20%">🎯<br><b>Custom Clean</b><br>自定义清理</td>
<td align="center" width="20%">📊<br><b>Before & After</b><br>前后对比验证</td>
</tr>
</table>

- **🌐 Bilingual / 中英双语** — Auto-detects user language / 自动识别用户语言
- **🤖 Agent-Agnostic / 通用兼容** — Works with any AI agent platform / 适用于任何 AI Agent 平台
- **🔒 No installation needed / 无需安装** — Just a single SKILL.md file / 只需一个 SKILL.md 文件

---

## 🤖 Compatible with / 适用于

| Platform / 平台 | Status / 状态 | How to use / 使用方式 |
|----------------|--------------|---------------------|
| **TRAE Work** | ✅ Native | Put in `.trae/skills/` |
| **Claude** (Anthropic) | ✅ | Use as system prompt / MCP |
| **GPT-4o / ChatGPT** | ✅ | Use as custom prompt / Custom GPT |
| **Gemini** (Google) | ✅ | Use as system instruction |
| **DeepSeek** | ✅ | Use as system prompt |
| **Qwen** (通义千问) | ✅ | Use as system prompt |
| **Doubao** (豆包) | ✅ | Use as system prompt |
| **Any LLM with tool use** | ✅ | Use SKILL.md as system prompt |

---

## 🚀 Quick Start / 快速开始

### 3 steps to clean your C drive / 3 步清理 C 盘

1. **Grab the skill / 获取技能**
   ```bash
   # Clone the repo / 克隆仓库
   git clone https://github.com/Zhu0512-ops/c-disk-cleaner.git
   ```
   Or just copy the content of `SKILL.md`.
   或者直接复制 `SKILL.md` 的内容。

2. **Load into your AI agent / 加载到 AI Agent**
   - **TRAE**: Put in `.trae/skills/c-disk-cleaner/`
   - **Other LLMs**: Paste `SKILL.md` as a system prompt

3. **Ask to clean up / 让它清理**
   > "帮我清理一下 C 盘" / "Clean up my C drive"

That's it! 🎉

---

## 🎬 Demo / 效果演示

### What the output looks like / 输出效果示例

```
🧹 C Drive Cleaner / C 盘清理工具
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 C 盘总览: 总容量 375 GB, 已用 202 GB (54%), 剩余 173 GB

🔍 扫描完成，结果如下：

✅ 低风险候选项（需确认）：
  • 用户临时文件     3.9 GB
  • Windows 临时文件   274 MB
  • Edge 缓存          379 MB
  • Chrome 缓存         16 MB
  • 缩略图缓存         314 MB
  • npm 缓存          123 MB
  ──────────────────────────
  预计可处理：约 5.0 GB

⚠️ 个人文件（默认只读）：
  • 下载文件夹      25.2 GB （含 MATLAB 安装包等）
  • 回收站            0 MB

💡 已展示清理计划：
  [1] 确认后快速清理 — 仅处理上面已校验的低风险候选项
  [2] 自定义处理 — 逐项查看路径、风险与影响
```

---

## 📂 What It Cleans / 清理内容

### ✅ Category A: Low-Risk Candidates / A 类：低风险候选项

These are typically regenerable temporary files and caches. They still require a current scan, exact-path review, and explicit confirmation before cleanup.
这些通常是可再生的临时文件与缓存，但清理前仍必须进行当前扫描、精确路径核对并获得明确确认。

| # | Item / 项目 | Location / 位置 | Typical Size / 通常大小 |
|---|------------|----------------|----------------------|
| 1 | User Temp Files / 用户临时文件 | `%TEMP%` | 几百 MB ~ 几 GB |
| 2 | Windows Temp Files / Windows 临时文件 | `C:\Windows\Temp` | 几十 MB ~ 几百 MB |
| 3 | Edge Browser Cache / Edge 缓存 | 已校验的 Profile 缓存子目录 | 几百 MB ~ 几 GB |
| 4 | Chrome Browser Cache / Chrome 缓存 | 已校验的 Profile 缓存子目录 | 几百 MB ~ 几 GB |
| 5 | Firefox Browser Cache / Firefox 缓存 | 已校验的 `cache2` 目录 | 几百 MB |
| 6 | Thumbnail Cache / 缩略图缓存 | `thumbcache_*.db` | 几十 MB ~ 几百 MB |
| 7 | npm Cache / npm 包缓存 | `npm config get cache` 返回的目录 | 几十 MB ~ 几 GB |

### ⚠️ Category B: Personal Files / B 类：个人文件

These contain personal data and are read-only by default. Before any action, the skill must show every path and risk, obtain explicit confirmation, verify a backup, and use Windows Recycle Bin instead of permanent deletion.
这些包含个人数据，默认只读。任何操作前，技能必须展示每条路径与风险、取得明确确认、验证备份，并使用 Windows 回收站而非永久删除。

| # | Item / 项目 | Location / 位置 |
|---|------------|----------------|
| 9 | Recycle Bin / 回收站 | Windows Recycle Bin interface / Windows 回收站界面 |
| 10 | Downloads Folder / 下载文件夹 | `%USERPROFILE%\Downloads` |
| 11 | Desktop Files / 桌面文件 | `%USERPROFILE%\Desktop` |

### 🔍 Category C: Deep Scan / C 类：深度扫描

For when you need more space — finds hidden space hogs.
需要更多空间时使用 — 找出隐藏的空间大户。

| # | Item / 项目 | Location / 位置 |
|---|------------|----------------|
| 12 | AppData Large Folders / AppData 大文件夹 | `%LOCALAPPDATA%`, `%APPDATA%`（仅报告） |
| 13 | Crash Dumps / 系统错误转储 | `CrashDumps`, `Minidump`（逐文件人工复核） |
| 14 | Windows Update Cache / 更新缓存 | `SoftwareDistribution\Download`（仅报告；使用官方流程处理） |
| 15 | Prefetch / 预读取文件 | `C:\Windows\Prefetch`（仅报告；不纳入快速清理） |

### ❌ Category D: Do NOT Touch / D 类：请勿触碰

These can break your system. Use official Windows tools instead.
手动删除这些可能破坏系统，请用 Windows 官方工具。

- `WinSxS` — Use Disk Cleanup or DISM / 请使用磁盘清理或 DISM
- System Restore Points / 系统还原点 — Use System Protection / 用系统保护管理
- `hiberfil.sys` — Manage through hibernation settings / 通过休眠设置管理
- `C:\Windows\Prefetch` and Windows Update cache — Report only; this skill never deletes them / Prefetch 与 Windows 更新缓存仅报告；本技能绝不删除

---

## 🛡️ Safety First / 安全第一

> **Rule #1: Scan first and show an exact cleanup plan. / 先扫描并展示精确清理计划。**
>
> **Rule #2: Every cleanup, including Quick Clean, needs specific confirmation. / 每次清理（包括快速清理）都需要针对清单的明确确认。**
>
> **Rule #3: Personal files are read-only by default; any action requires a verified backup and Windows Recycle Bin. / 个人文件默认只读；任何操作都需要已验证备份并使用 Windows 回收站。**
>
> **Rule #4: WinSxS, Prefetch, Windows Update cache, restore points, and hiberfil.sys are never deleted by this skill. / 本技能绝不删除 WinSxS、Prefetch、Windows 更新缓存、还原点或 hiberfil.sys。**

See [SKILL.md → Safety Rules](SKILL.md#safety-rules-must-follow--安全准则必须遵守) for the full safety policy.

完整安全准则请查看 [SKILL.md → 安全准则](SKILL.md#safety-rules-must-follow--安全准则必须遵守)。

---

## ❓ FAQ / 常见问题

<details>
<summary><b>Is this safe? Will it delete important files? / 安全吗？会不会删掉重要文件？</b></summary>
<br>
Yes, it's safe. The AI follows strict rules:
<br>
安全的。AI 严格遵守以下规则：
<br><br>
• Category A items are low-risk candidates, not blanket deletion targets. The skill validates each current path and requires confirmation before cleanup.<br>
  A 类项目是低风险候选项，而非可直接删除的目标。技能会校验本次路径，并在清理前取得确认。<br>
• Category B items (Downloads, Desktop, Recycle Bin) are read-only by default. Any action needs an itemized review, explicit confirmation, verified backup, and Windows Recycle Bin.<br>
  B 类项目（下载、桌面、回收站）默认只读。任何操作都需逐项复核、明确确认、验证备份并使用 Windows 回收站。<br>
• Category D items (WinSxS, Prefetch, Windows Update cache, restore points, hiberfil.sys) are never deleted by this skill.<br>
  D 类项目（WinSxS、Prefetch、Windows 更新缓存、还原点、休眠文件）绝不由本技能删除。
</details>

<details>
<summary><b>How much space can I free? / 能释放多少空间？</b></summary>
<br>
It depends on how long it's been since your last cleanup. Typical results:
<br>
取决于你上次清理到现在多久。通常情况：
<br><br>
• Confirmed low-risk cleanup (Category A): results vary by device and current cache size.<br>
  经确认的低风险清理（A 类）：效果取决于设备与当前缓存大小。<br>
• Personal files and crash dumps are reviewed item by item, not included in a bulk-cleanup estimate.<br>
  个人文件与崩溃转储需逐项复核，不纳入批量清理空间预估。<br><br>
The biggest storage gains may be in Downloads or diagnostic files, but they require careful, specific review.
下载目录或诊断文件可能占用较多空间，但必须谨慎逐项复核。
</details>

<details>
<summary><b>Do I need to install anything? / 需要安装什么吗？</b></summary>
<br>
No installation needed. Just the SKILL.md file + an AI agent that can run PowerShell commands.
<br>
不需要安装任何东西。只需要 SKILL.md 文件 + 一个能运行 PowerShell 命令的 AI Agent。
</details>

<details>
<summary><b>Why use AI instead of Disk Cleanup / CCleaner? / 为什么用 AI 而不是磁盘清理/CCleaner？</b></summary>
<br>
• You can <b>ask questions</b> — "Is this file safe to delete?" "What's taking up space?"<br>
  你可以<b>提问</b> — "这个文件删了安全吗？""什么东西占空间？"<br>
• It <b>explains</b> what it found, the associated risk, and the expected side effects<br>
  它会<b>解释</b>找到了什么、相关风险与预期影响<br>
• You're <b>in control</b> — confirm the displayed low-risk plan or review individual items<br>
  你<b>说了算</b> — 确认展示的低风险计划，或逐项复核<br>
• It's <b>transparent</b> — you see the exact validated targets before any cleanup<br>
  全程<b>透明</b> — 清理前会展示已校验的精确目标
</details>

---

## 📁 Project Structure / 项目结构

```
c-disk-cleaner/
├── .github/                    # GitHub templates / GitHub 模板
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── PULL_REQUEST_TEMPLATE.md
├── .gitignore
├── CONTRIBUTING.md             # Contribution guide / 贡献指南
├── LICENSE                     # MIT License
├── README.md                   # This file / 本文件
└── SKILL.md                    # ⭐ The skill / 技能核心文件
```

---

## 🤝 Contributing / 贡献

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

欢迎贡献！详情请参阅 [CONTRIBUTING.md](CONTRIBUTING.md)。

We welcome contributions that:
- Add support for more AI agent platforms / 添加更多平台支持
- Add more cleanable categories / 添加更多可清理类别
- Improve safety measures / 改进安全措施
- Fix bugs and improve documentation / 修复 Bug 和改进文档

---

## 📄 License / 许可证

[MIT](LICENSE) © 29429

---

## ⭐ Star History / Star 增长

If you find this useful, give it a star! ⭐

如果你觉得有用，点个 Star 支持一下！

---

<div align="center">

**Made with ❤️ by 29429**

[GitHub](https://github.com/Zhu0512-ops/c-disk-cleaner) · [Issues](https://github.com/Zhu0512-ops/c-disk-cleaner/issues) · [Releases](https://github.com/Zhu0512-ops/c-disk-cleaner/releases)

</div>
