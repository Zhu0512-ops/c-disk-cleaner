# c-disk-cleaner

> A universal AI agent skill for safely scanning and cleaning up junk files on Windows C drive. Works with any LLM agent that supports custom skills / prompts.

> 一个通用的 AI Agent 技能，用于安全扫描和清理 Windows C 盘垃圾文件。适用于所有支持自定义 skill / prompt 的 LLM Agent。

---

## English | 中文

---

## 📖 Introduction / 简介

**c-disk-cleaner** is a universal AI agent skill that comprehensively scans your Windows C drive for junk files, cache files, and large files, assesses the safety of each item, provides a clear space usage report, and performs safe cleanup based on user confirmation.

**c-disk-cleaner** 是一个通用的 AI Agent 技能，全面扫描你的 Windows C 盘中的垃圾文件、缓存文件和大文件，评估每项的安全性，提供清晰的空间占用报告，并根据用户确认执行安全清理。

### Compatible with / 适用于

| Agent / Platform | Support / 支持 | How to use / 使用方式 |
|-----------------|---------------|---------------------|
| TRAE Work | ✅ Native | Put in `.trae/skills/` |
| Claude (Anthropic) | ✅ | Use as custom prompt / MCP |
| GPT-4o / ChatGPT | ✅ | Use as custom prompt / custom GPT |
| Gemini (Google) | ✅ | Use as custom prompt |
| DeepSeek | ✅ | Use as system prompt |
| Any LLM with tool use | ✅ | Use SKILL.md as system prompt |
| 任何支持工具调用的大模型 | ✅ | 将 SKILL.md 作为系统提示词 |

### Features / 功能特性

- 🔍 **Comprehensive Scan / 全面扫描** — Scans 14+ categories of cleanable files / 扫描 14+ 类可清理文件
- 🛡️ **Safety First / 安全第一** — 4-level safety classification, never delete without confirmation / 四级安全分级，未经确认绝不删除
- ⚡ **Quick Clean / 快速清理** — One-click safe cleanup mode / 一键安全清理模式
- 🎯 **Custom Clean / 自定义清理** — Flexible item-by-item selection / 灵活的逐项选择
- 📊 **Before & After / 前后对比** — Verifies actual freed space after cleanup / 清理后验证实际释放空间
- 🌐 **Bilingual / 中英双语** — Supports both English and Chinese output / 支持中英文输出
- 🤖 **Agent-Agnostic / 通用兼容** — Works with any AI agent platform / 适用于任何 AI Agent 平台

---

## 🚀 Installation / 安装

### Prerequisites / 前置要求

- Windows operating system / Windows 操作系统
- PowerShell 5.0+
- An AI agent that can execute shell commands / 一个能执行 shell 命令的 AI Agent

### For TRAE Work / TRAE Work 用户

1. Copy the `c-disk-cleaner/` directory into your workspace's `.trae/skills/` folder.
   将 `c-disk-cleaner/` 目录复制到你的工作区的 `.trae/skills/` 文件夹中。

2. Restart TRAE or reload skills.
   重启 TRAE 或重新加载 skills。

### For Other AI Agents / 其他 AI Agent 用户

**Option 1: System Prompt / 方案一：系统提示词**

Copy the content of `SKILL.md` into your agent's system prompt or instructions.

将 `SKILL.md` 的内容复制到你的 Agent 的系统提示词或指令中。

**Option 2: Custom Skill / 方案二：自定义 Skill**

If your agent platform supports custom skills, import `SKILL.md` as a skill file.

如果你的 Agent 平台支持自定义 skill，将 `SKILL.md` 作为 skill 文件导入。

**Option 3: Manual Reference / 方案三：手动参考**

Simply keep `SKILL.md` handy and reference the workflow when you need to clean up C drive.

直接在需要清理 C 盘时参考 `SKILL.md` 中的流程。

---

## 💡 Usage / 使用

Simply ask your AI agent something like:

只需向你的 AI Agent 提问类似以下内容：

**English:**
- "Clean up my C drive"
- "My C drive is full, help me free up space"
- "What files can I delete from C drive?"
- "Scan junk files on C drive"

**中文：**
- 「帮我清理一下 C 盘」
- 「C 盘满了，帮我释放点空间」
- 「C 盘哪些文件可以删？」
- 「扫描一下 C 盘的垃圾文件」

### Workflow / 工作流程

```
1. C Drive Overview → Show total/used/free space
   C 盘总览 → 展示总容量/已用/剩余
2. Scan Phase → Scan 14+ categories of files
   扫描阶段 → 扫描 14+ 类文件
3. Report Phase → Clear table with safety ratings
   报告阶段 → 带安全等级的清晰表格
4. Confirm Phase → Choose Quick Clean or Custom Clean
   确认阶段 → 选择快速清理或自定义清理
5. Cleanup Phase → Execute selected cleanup items
   清理阶段 → 执行选定的清理项目
6. Verification → Re-scan and show space freed
   验证阶段 → 重新扫描并展示释放空间
```

---

## 📂 What It Cleans / 清理内容

### Category A: Safe to Clean (Recommended) / A 类：可安全清理（推荐）

| # | Item / 项目 | Path / 路径 |
|---|------------|-------------|
| 1 | User Temp Files / 用户临时文件 | `%TEMP%` |
| 2 | Windows Temp Files / Windows 临时文件 | `C:\Windows\Temp` |
| 3 | Edge Browser Cache / Edge 浏览器缓存 | `%LOCALAPPDATA%\Microsoft\Edge\...\Cache` |
| 4 | Chrome Browser Cache / Chrome 浏览器缓存 | `%LOCALAPPDATA%\Google\Chrome\...\Cache` |
| 5 | Firefox Browser Cache / Firefox 浏览器缓存 | `%LOCALAPPDATA%\Mozilla\Firefox\...\cache2` |
| 6 | Thumbnail Cache / 缩略图缓存 | `thumbcache_*.db` |
| 7 | npm Cache / npm 缓存 | `npm config get cache` |
| 8 | Prefetch Files / 预读取文件 | `C:\Windows\Prefetch` |

### Category B: Needs Confirmation / B 类：需用户确认

| # | Item / 项目 | Path / 路径 |
|---|------------|-------------|
| 9 | Recycle Bin / 回收站 | `C:\$Recycle.Bin` |
| 10 | Downloads Folder / 下载文件夹 | `%USERPROFILE%\Downloads` |
| 11 | Desktop Files / 桌面文件 | `%USERPROFILE%\Desktop` |

### Category C: Deep Scan / C 类：深度扫描

| # | Item / 项目 | Path / 路径 |
|---|------------|-------------|
| 12 | AppData Large Folders / AppData 大文件夹 | `%LOCALAPPDATA%`, `%APPDATA%` |
| 13 | Crash Dumps / 系统错误转储 | `CrashDumps`, `Minidump` |
| 14 | Windows Update Cache / Windows 更新缓存 | `SoftwareDistribution\Download` |

### Category D: Do NOT Delete Manually / D 类：请勿手动删除

- `WinSxS` — Use Disk Cleanup tool / 请使用磁盘清理工具
- System Restore Points / 系统还原点 — Use System Protection / 通过系统保护管理
- `hiberfil.sys` — Disable hibernation to remove / 关闭休眠才能删除

---

## ⚠️ Safety Notes / 安全说明

1. **Always scan before cleaning / 先扫描后清理** — Results are shown before any deletion / 删除前先展示结果
2. **No auto-deletion for B-category / B 类不自动删除** — User must explicitly confirm / 用户必须明确确认
3. **In-use files are skipped / 跳过使用中的文件** — Files locked by running programs are skipped / 被程序锁定的文件会跳过
4. **WinSxS is never touched / 绝不碰 WinSxS** — Manual deletion can break your system / 手动删除可能破坏系统
5. **Browser cache note / 浏览器缓存说明** — Web pages may load slower once after cleanup / 清理后网页首次加载可能稍慢

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
└── SKILL.md                    # Skill definition / 技能定义文件
```

---

## 🤝 Contributing / 贡献

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

欢迎贡献！详情请参阅 [CONTRIBUTING.md](CONTRIBUTING.md)（英文）。

We welcome contributions that:
- Add support for more AI agent platforms
- Add more cleanable categories
- Improve safety measures
- Fix bugs and improve documentation

我们欢迎以下类型的贡献：
- 添加更多 AI Agent 平台支持
- 添加更多可清理类别
- 改进安全措施
- 修复 Bug 和改进文档

---

## 📄 License / 许可证

[MIT](LICENSE) © 29429

---

## 🔗 Related / 相关链接

- [Issue Tracker / 问题反馈](https://github.com/Zhu0512-ops/c-disk-cleaner/issues)
- [Releases / 版本发布](https://github.com/Zhu0512-ops/c-disk-cleaner/releases)

---

<p align="center">
Made with ❤️ by 29429
</p>
