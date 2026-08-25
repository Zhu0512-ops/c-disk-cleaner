---
name: "c-disk-cleaner"
description: "This skill should be used to scan Windows C drive storage and conduct user-confirmed, low-risk cleanup of approved temporary files and caches when users mention C drive cleanup, low disk space, junk files, or cache files."
platforms: ["TRAE", "Claude", "GPT-4", "Gemini", "DeepSeek", "Any LLM with tool use"]
version: "1.2.0"
agent_created: true
---

# C Drive Cleaner / C 盘清理工具

> A universal AI agent skill for safely scanning and cleaning up junk files on Windows C drive.
> 一个通用的 AI Agent 技能，用于安全扫描和清理 Windows C 盘垃圾文件。

---

## English | 中文

---

## Overview / 概述

A universal AI agent skill that comprehensively scans the Windows C drive for cleanable junk files, cache files, and large files, assesses the safety of each item, provides a clear space usage report, and performs safe cleanup based on user confirmation.

一个通用的 AI Agent 技能，全面扫描 Windows C 盘中可清理的垃圾文件、缓存文件和大文件，评估每项的安全性，提供清晰的空间占用报告，并根据用户确认执行安全清理。

**Compatible with / 适用于**: TRAE Work, Claude, GPT-4o, Gemini, DeepSeek, and any LLM agent that can execute shell commands.
**适用于**: TRAE Work、Claude、GPT-4o、Gemini、DeepSeek，以及任何能执行 shell 命令的 LLM Agent。

---

## When to Invoke / 触发场景

**Auto-trigger when user mentions:**
**用户提到以下内容时自动触发：**

- Clean up C drive, C drive full, C drive low on space, C drive red
  清理 C 盘、C 盘满了、C 盘空间不足、C 盘变红
- Clean junk files, temp files, cache files
  清理垃圾文件、临时文件、缓存文件
- Free up disk space, computer is slow, computer is laggy
  释放磁盘空间、电脑卡、电脑变慢
- What files can I delete, what's taking up space on C drive
  哪些文件可以删除、C 盘什么东西没用
- Disk cleanup, system slimming
  磁盘清理、系统瘦身

---

## Output Language / 输出语言

Automatically switches based on user input language. Defaults to English.

根据用户输入语言自动切换。默认英文。

---

## Workflow / 工作流程

### Step 1: C Drive Overview / 第一步：C 盘总览

First, show the overall C drive space status to give the user a global picture.

首先展示 C 盘整体空间情况，让用户有全局概念。

```powershell
$drive = Get-PSDrive C
$usedGB = [math]::Round($drive.Used/1GB, 2)
$freeGB = [math]::Round($drive.Free/1GB, 2)
$totalGB = [math]::Round(($drive.Used + $drive.Free)/1GB, 2)
$usedPercent = [math]::Round($drive.Used / ($drive.Used + $drive.Free) * 100, 1)
```

**Output format / 输出格式示例：**
> C Drive Overview / C 盘总览: Total / 总容量 **XXX GB**, Used / 已用 **XXX GB** (XX.X%), Free / 剩余 **XXX GB**

---

### Step 2: Scan Phase / 第二步：扫描阶段

Scan the following items sequentially using PowerShell, recording the size of each. All paths use `$env:` environment variable syntax.

使用 PowerShell 依次扫描以下项目，记录每项的大小。所有路径使用 `$env:` 环境变量写法。

#### Category A: Low-Risk Candidates (Recommended After Confirmation) / A 类 - 低风险候选项（确认后推荐）

| # | Item / 项目 | Path / 路径 | Scan Method / 扫描方式 | Execution notes / 执行说明 |
|---|------------|-------------|----------------------|---------------------------|
| 1 | User Temp Files / 用户临时文件 | `$env:TEMP` | Recursive directory size / 递归计算目录大小 | Only delete verified children; skip reparse points / 仅删除经校验的子项；跳过重解析点 |
| 2 | Windows Temp Files / Windows 临时文件 | `C:\Windows\Temp` | Recursive directory size / 递归计算目录大小 | Require explicit confirmation; skip files in use / 需明确确认；跳过占用文件 |
| 3 | Edge Browser Cache / Edge 浏览器缓存 | Approved Cache, Code Cache, GPUCache folders under browser profiles | Traverse profiles / 遍历配置文件 | Ask user to close browser; never delete User Data or profile root / 提示关闭浏览器；绝不删除 User Data 或配置根目录 |
| 4 | Chrome Browser Cache / Chrome 浏览器缓存 | Approved Cache, Code Cache, GPUCache folders under browser profiles | Traverse profiles / 遍历配置文件 | Ask user to close browser; never delete User Data or profile root / 提示关闭浏览器；绝不删除 User Data 或配置根目录 |
| 5 | Firefox Browser Cache / Firefox 浏览器缓存 | `$env:LOCALAPPDATA\Mozilla\Firefox\Profiles\*\cache2` | Traverse profiles / 遍历配置文件 | Delete only cache2 contents after path validation / 校验路径后仅删除 cache2 内容 |
| 6 | Thumbnail Cache / 缩略图缓存 | `$env:LOCALAPPDATA\Microsoft\Windows\Explorer\thumbcache_*.db` | Sum matching files / 统计匹配文件 | Delete only exact thumbcache files / 仅删除精确匹配的 thumbcache 文件 |
| 7 | npm Cache / npm 缓存 | via `npm config get cache` | Recursive directory size / 递归计算目录大小 | Show cache path and require separate confirmation / 展示缓存路径并单独确认 |

#### Category B: Needs User Confirmation / B 类 - 需用户确认项

| # | Item / 项目 | Path / 路径 | Notes / 说明 |
|---|------------|-------------|-------------|
| 9 | Recycle Bin / 回收站 | Windows Recycle Bin interface / Windows 回收站界面 | Calculate visible size through supported interface; explain impact / 通过受支持界面计算可见大小并说明影响 |
| 10 | Downloads Folder / 下载文件夹 | `$env:USERPROFILE\Downloads` | Total size + top 10 largest files / 总大小 + 最大 10 个文件 |
| 11 | Desktop Files / 桌面文件 | `$env:USERPROFILE\Desktop` | Total size + top 5 largest files / 总大小 + 最大 5 个文件 |

#### Category C: Deep Scan and Manual Review / C 类 - 深度扫描与人工复核

| # | Item / 项目 | Path / 路径 | Notes / 说明 |
|---|------------|-------------|-------------|
| 12 | AppData Large Folders / AppData 大文件夹 | `$env:LOCALAPPDATA` and `$env:APPDATA` | List top 10 subfolders only; do not delete automatically / 仅列出前 10 个目录，不自动删除 |
| 13 | Crash Dumps / 系统错误转储 | `$env:LOCALAPPDATA\CrashDumps`, `C:\Windows\Minidump` | List filename, size, and modified time; keep recent diagnostics unless user explicitly selects files / 列出名称、大小、修改时间；除非用户明确选择，否则保留近期诊断文件 |
| 14 | Windows Update Cache / 更新缓存 | `C:\Windows\SoftwareDistribution\Download` | Report only; direct cleanup requires a separate, official Windows Update service workflow / 仅报告；直接清理需单独采用官方 Windows Update 服务流程 |
| 15 | Prefetch Files / 预读取文件 | `C:\Windows\Prefetch` | Report only; do not include in Quick Clean because benefits are limited / 仅报告；收益有限，不纳入快速清理 |

#### Category D: Never Delete Through This Skill / D 类 - 本技能绝不删除

- `C:\Windows\WinSxS` — Use the official Disk Cleanup or DISM workflow / 使用官方磁盘清理或 DISM 流程
- System Restore Points / 系统还原点 — Manage through System Protection / 通过系统保护界面管理
- Hibernation file `hiberfil.sys` — Manage only by changing hibernation settings / 仅能通过休眠设置管理
- Any drive root, Windows system/configuration directory, junction, symbolic link, or reparse point / 任何磁盘根目录、Windows 系统/配置目录、联接、符号链接或重解析点

---

### Step 3: Report Phase / 第三步：报告阶段

Present scan results in a clear Markdown table:

将扫描结果整理成清晰的 Markdown 表格：

| Category / 分类 | Item / 项目 | Size / 大小 | Safety / 安全等级 | Notes / 说明 |
|----------------|------------|-------------|------------------|-------------|
| Low-Risk Candidate / 低风险候选 | User Temp Files / 用户临时文件 | XXX MB | ✅ Confirm required / 需确认 | Program temp files; validated path required / 程序临时文件；须校验路径 |
| Personal Files / 个人文件 | Downloads / 下载文件夹 | X.XX GB | ⚠️ Read-only by default / 默认只读 | XX files, largest is XXX; backup and itemized confirmation required / 含 XX 个文件，最大的是 XXX；需备份与逐项确认 |
| Manual Review / 人工复核 | Crash Dumps / 系统错误转储 | XXX MB | 🔍 Review only / 仅复核 | List metadata; do not delete automatically / 列出元数据；不自动删除 |
| ... | ... | ... | ... | ... |

Add at the end / 末尾加上：
> Total confirmed low-risk cleanup candidates / 经确认可处理的低风险候选项合计：**X.XX GB**
> For personal files or deep-scan results, review the displayed paths and risks before requesting a specific action.
> 对个人文件或深度扫描结果，请先核对展示的路径与风险，再请求具体操作。

---

### Step 4: Confirmation Phase / 第四步：确认阶段

After presenting results, offer two cleanup modes:

展示扫描结果后，提供两种清理模式：

**Mode 1: Quick Clean (Recommended) / 模式一：快速清理（推荐）**
- Include only the approved Category A low-risk candidates that were found by the current scan / 仅包含本次扫描发现且允许处理的 A 类低风险候选项
- Show exact categories, paths, estimated reclaimable space, and expected side effects before proceeding / 执行前展示确切类别、路径、预计释放空间和预期影响
- Require an explicit confirmation for this specific list; never treat a generic “clean C drive” request as permission to delete / 必须针对本次清单获得明确确认；不得把泛泛的“清理 C 盘”视为删除授权

**Mode 2: Custom Clean / 模式二：自定义清理**
- Let the user choose individual items or categories from the report / 让用户从报告中选择具体项目或类别
- Re-confirm each Category B item with its full path, size, and risk / 对每个 B 类项目展示完整路径、大小与风险后再次确认
- For personal files, create and verify a backup before any move or deletion; use Windows Recycle Bin rather than permanent deletion / 对个人文件，移动或删除前先创建并验证备份；使用 Windows 回收站而不是永久删除

Default recommendation: Scan first, then use Quick Clean only after confirming the displayed plan / 默认建议：先扫描，再在确认展示的计划后使用快速清理。

---

### Step 5: Execute Cleanup / 第五步：执行清理

Execute only the exact, confirmed items sequentially. Before every destructive operation, validate the resolved absolute path against the approved scan result and an allowlist. Never accept a user-supplied or dynamically discovered path as a deletion target without validation. Report completed, skipped, and failed counts after each category.

仅按顺序执行已明确确认的精确项目。每个破坏性操作前，将解析后的绝对路径与已批准的扫描结果及允许清单进行校验。未校验前，不得将用户提供或动态发现的路径作为删除目标。每个类别结束后汇报已完成、已跳过和失败数量。

#### Mandatory Execution Guardrails / 强制执行护栏

- Use a dry-run report before actual cleanup: show each target path, size, and category / 实际清理前先输出演练报告：展示每个目标路径、大小和类别。
- Reject a target when it is a drive root, system/configuration directory, junction, symbolic link, reparse point, or outside the approved allowlist / 当目标为磁盘根目录、系统/配置目录、联接、符号链接、重解析点，或不在允许清单中时拒绝执行。
- Use `-LiteralPath`; enumerate and handle only direct children of an already validated directory / 使用 `-LiteralPath`；只枚举和处理已校验目录的直接子项。
- Do not suppress all errors. Capture up to five representative errors and include them in the summary / 不得吞掉所有错误。记录最多五条代表性错误并纳入汇总。
- Skip files in use, access-denied targets, and all reparse points; do not retry with elevated or forced workarounds / 跳过被占用、拒绝访问的目标及所有重解析点；不得以提权或强制方式重试。
- Limit personal-file moves or deletions to ten items per batch. Stop immediately on any failure / 个人文件移动或删除每批最多十项；发生任何失败立即停止。

#### Approved Cleanup Operations / 可批准的清理操作

**1. User Temp Files and Windows Temp Files / 用户临时文件与 Windows 临时文件：**
- After confirmation, remove only validated direct children from the scanned temp directories; do not use wildcard or root-recursive deletion / 确认后，仅删除扫描到的临时目录中已校验的直接子项；不得使用通配符或根目录递归删除。
- Preserve a per-item result list with success, skip, or failure status / 保存每个项目的成功、跳过或失败状态。

**2. Browser Cache / 浏览器缓存：**
- Request that the user closes the browser first. Restrict targets to validated `Cache`, `Code Cache`, `GPUCache`, or Firefox `cache2` folders beneath a detected profile / 先提示用户关闭浏览器。目标仅限于已检测配置文件下经校验的 `Cache`、`Code Cache`、`GPUCache` 或 Firefox `cache2` 目录。
- Never delete a browser profile, `User Data`, history, cookies, bookmarks, extensions, or login data / 绝不删除浏览器配置文件、`User Data`、历史记录、Cookie、书签、扩展或登录数据。

**3. Thumbnail Cache / 缩略图缓存：**
- Delete only explicitly enumerated files matching `thumbcache_*.db` in the validated Explorer cache directory / 仅删除在已校验 Explorer 缓存目录中显式枚举、且匹配 `thumbcache_*.db` 的文件。

**4. npm Cache / npm 缓存：**
- Run `npm config get cache` first. Show the resolved path and seek separate confirmation before invoking `npm cache clean --force` / 先运行 `npm config get cache`。展示解析后的路径，并在执行 `npm cache clean --force` 前单独确认。

**5. Recycle Bin and Personal Files / 回收站与个人文件：**
- Do not enumerate or manipulate `C:\$Recycle.Bin` directly. Use the Windows Recycle Bin interface only after a specific confirmation / 不得直接枚举或操作 `C:\$Recycle.Bin`。仅在具体确认后通过 Windows 回收站接口操作。
- For Downloads, Desktop, Documents, Home, or other personal directories: scan read-only first; display every affected full path, size, modification time, and risk; create and verify a backup before action; move files to Windows Recycle Bin instead of permanent deletion / 对下载、桌面、文档、Home 或其他个人目录：先只读扫描；展示每个受影响项目的完整路径、大小、修改时间和风险；操作前创建并验证备份；将文件移至 Windows 回收站而非永久删除。
- Never batch-delete an entire personal directory / 禁止批量删除整个个人目录。

---

### Step 6: Verification & Summary / 第六步：验证与汇总

After cleanup, **re-scan C drive total space** to verify results:

清理完成后，**重新扫描 C 盘总空间**，验证清理效果：

1. Re-check C drive used/free space / 重新获取 C 盘已用/剩余空间
2. Calculate actual total space freed / 计算本次实际释放的总空间
3. Generate before/after comparison table / 生成清理前后对比表

**Output format / 输出格式示例：**

| Item / 项目 | Before / 清理前 | After / 清理后 | Freed / 释放空间 |
|------------|---------------|---------------|-----------------|
| User Temp Files / 用户临时文件 | XXX MB | XX MB | XXX MB |
| ... | ... | ... | ... |
| **C Drive Free / C 盘剩余** | **XXX GB** | **XXX GB** | **X.XX GB** |

---

## Safety Rules (MUST Follow) / 安全准则（必须遵守）

1. **Scan before cleanup / 先扫描后清理** — Conduct a read-only scan and show the exact plan before any cleanup action / 任何清理前先只读扫描并展示精确计划。
2. **Require specific confirmation / 需要具体确认** — Obtain confirmation for the displayed categories and targets; a generic cleanup request is never deletion authorization / 必须针对展示的类别和目标获得确认；泛化清理请求绝不是删除授权。
3. **Validate every path / 校验每条路径** — Resolve to an absolute path, verify it belongs to the allowlist and scan result, and reject roots, system/configuration directories, links, and reparse points / 解析为绝对路径，验证其属于允许清单和扫描结果，拒绝根目录、系统/配置目录、链接和重解析点。
4. **Protect personal files / 保护个人文件** — Treat Downloads, Desktop, Documents, Home, and their descendants as read-only by default. Before any action, warn in bold: **“⚠️ 此操作非常危险，可能导致不可逆的数据丢失！”**, list every file and risk, obtain explicit confirmation, create and verify a backup, and use Windows Recycle Bin / 默认将下载、桌面、文档、Home 及其子项设为只读。操作前必须以粗体警告、列出文件与风险、取得明确确认、创建并验证备份，并使用 Windows 回收站。
5. **Use small, observable batches / 小批量且可观察** — Limit personal-file operations to ten items per batch, verify each batch, and stop on any failure / 个人文件操作每批最多十项，逐批验证，任何失败即停止。
6. **Skip unsafe or unavailable targets / 跳过不安全或不可用目标** — Skip files in use, access-denied files, and reparse points. Do not force retries or elevate permissions / 跳过被占用、拒绝访问的文件及重解析点。不得强制重试或提权。
7. **Keep error evidence / 保留错误证据** — Do not hide all errors. Summarize successes, skips, failures, and up to five representative errors / 不得隐藏全部错误。汇总成功、跳过、失败数量及最多五条代表性错误。
8. **Never touch protected system content / 禁止触碰受保护系统内容** — Never delete WinSxS, restore points, hiberfil.sys, Prefetch, or Windows Update cache through this skill / 本技能绝不删除 WinSxS、还原点、hiberfil.sys、Prefetch 或 Windows 更新缓存。
9. **Explain side effects / 说明副作用** — Browser caches may make the first page load slower; npm cache cleanup can require later re-downloads / 浏览器缓存清理后首次加载可能变慢；npm 缓存清理后可能需要重新下载依赖。

---

## Helper Functions / 辅助工具函数

**Calculate folder size / 计算文件夹大小：**
```powershell
function Get-FolderSize($path) {
    if (Test-Path $path) {
        $size = (Get-ChildItem $path -Recurse -ErrorAction SilentlyContinue | Measure-Object -Property Length -Sum).Sum
        if (-not $size) { $size = 0 }
        return $size
    }
    return 0
}
```

**Format size display / 格式化大小显示：**
```powershell
function Format-Size($bytes) {
    if (-not $bytes) { $bytes = 0 }
    if ($bytes -ge 1GB) { return [math]::Round($bytes/1GB, 2).ToString() + " GB" }
    elseif ($bytes -ge 1MB) { return [math]::Round($bytes/1MB, 2).ToString() + " MB" }
    elseif ($bytes -ge 1KB) { return [math]::Round($bytes/1KB, 2).ToString() + " KB" }
    else { return $bytes.ToString() + " B" }
}
```

---

## Platform-Specific Notes / 平台特定说明

### TRAE Work
- Place this file in `.trae/skills/c-disk-cleaner/SKILL.md`
- 将此文件放在 `.trae/skills/c-disk-cleaner/SKILL.md`

### Claude / GPT-4o / Gemini / Other LLMs
- Use the full content of this file as a system prompt or custom instruction
- 将此文件的完整内容用作系统提示词或自定义指令
- Ensure the model has access to shell/terminal execution capabilities
- 确保模型具有 shell/终端执行能力

---

## Version / 版本

- **Version / 版本**: 1.2.0
- **Author / 作者**: 29429
- **License / 许可证**: MIT
- **Repository / 仓库**: https://github.com/Zhu0512-ops/c-disk-cleaner
