---
name: "c-disk-cleaner"
description: "Scans Windows C drive for junk files, caches, and large files; assesses safety and performs cleanup to free disk space. Invoke when user mentions C drive cleanup, freeing space, junk files, temp files, or C drive full."
platforms: ["TRAE", "Claude", "GPT-4", "Gemini", "DeepSeek", "Any LLM with tool use"]
version: "1.1.0"
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

#### Category A: Safe to Clean (Recommended) / A 类 - 可安全清理项（默认推荐清理）

| # | Item / 项目 | Path / 路径 | Scan Method / 扫描方式 |
|---|------------|-------------|----------------------|
| 1 | User Temp Files / 用户临时文件 | `$env:TEMP` | Recursive directory size / 递归计算目录大小 |
| 2 | Windows Temp Files / Windows 临时文件 | `C:\Windows\Temp` | Recursive directory size / 递归计算目录大小 |
| 3 | Edge Browser Cache / Edge 浏览器缓存 | `$env:LOCALAPPDATA\Microsoft\Edge\User Data\Default\Cache` | Recursive directory size / 递归计算目录大小 |
| 4 | Chrome Browser Cache / Chrome 浏览器缓存 | `$env:LOCALAPPDATA\Google\Chrome\User Data\Default\Cache` | Recursive directory size / 递归计算目录大小 |
| 5 | Firefox Browser Cache / Firefox 浏览器缓存 | `$env:LOCALAPPDATA\Mozilla\Firefox\Profiles\` → `cache2` | Traverse profiles / 遍历 profile 目录 |
| 6 | Thumbnail Cache / 缩略图缓存 | `$env:LOCALAPPDATA\Microsoft\Windows\Explorer\thumbcache_*.db` | Sum thumbcache files / 统计缩略图文件 |
| 7 | npm Cache / npm 缓存 | via `npm config get cache` | Recursive directory size / 递归计算目录大小 |
| 8 | Prefetch Files / 预读取文件 | `C:\Windows\Prefetch` | Directory size / 计算目录大小 |

#### Category B: Needs User Confirmation / B 类 - 需用户确认项

| # | Item / 项目 | Path / 路径 | Notes / 说明 |
|---|------------|-------------|-------------|
| 9 | Recycle Bin / 回收站 | `C:\$Recycle.Bin` | Calculate total size, explain impact / 计算总大小，说明影响 |
| 10 | Downloads Folder / 下载文件夹 | `$env:USERPROFILE\Downloads` | Total size + top 10 largest files / 总大小 + 最大 10 个文件 |
| 11 | Desktop Files / 桌面文件 | `$env:USERPROFILE\Desktop` | Total size + top 5 largest files / 总大小 + 最大 5 个文件 |

#### Category C: Deep Scan / C 类 - 深度扫描项

| # | Item / 项目 | Path / 路径 | Notes / 说明 |
|---|------------|-------------|-------------|
| 12 | AppData Large Folders / AppData 大文件夹 | `$env:LOCALAPPDATA` and `$env:APPDATA` | Top 10 subfolders by size / 占用空间前 10 的子文件夹 |
| 13 | Crash Dumps / 系统错误转储 | `$env:LOCALAPPDATA\CrashDumps`, `C:\Windows\Minidump` | System crash logs / 系统崩溃日志 |
| 14 | Windows Update Cache / Windows 更新缓存 | `C:\Windows\SoftwareDistribution\Download` | Update package cache / 更新安装包缓存 |

#### Category D: Do NOT Delete Manually / D 类 - 不建议手动删除

- `C:\Windows\WinSxS` — Windows component store, must use Disk Cleanup tool
  Windows 组件存储，必须用系统磁盘清理工具
- System Restore Points / 系统还原点 — Manage via System Protection / 通过系统保护界面管理
- Hibernation file `hiberfil.sys` — Disable hibernation to remove / 关闭休眠才能删除

---

### Step 3: Report Phase / 第三步：报告阶段

Present scan results in a clear Markdown table:

将扫描结果整理成清晰的 Markdown 表格：

| Category / 分类 | Item / 项目 | Size / 大小 | Safety / 安全等级 | Notes / 说明 |
|----------------|------------|-------------|------------------|-------------|
| Safe to Clean / 可安全清理 | User Temp Files / 用户临时文件 | XXX MB | ✅ Safe / 安全 | Program temp files / 程序运行临时文件 |
| Needs Confirm / 需确认 | Downloads / 下载文件夹 | X.XX GB | ⚠️ Confirm / 需确认 | XX files, largest is XXX / 含 XX 个文件，最大的是 XXX |
| ... | ... | ... | ... | ... |

Add at the end / 末尾加上：
> Total safe to clean / 总计可安全清理：**X.XX GB**
> For deep cleanup (Downloads, AppData large files, etc.), let me know.
> 如需深度清理（下载文件夹、AppData 大文件等），请告诉我。

---

### Step 4: Confirmation Phase / 第四步：确认阶段

After presenting results, offer two cleanup modes:

展示扫描结果后，提供两种清理模式：

**Mode 1: Quick Clean (Recommended) / 模式一：快速清理（推荐）**
- Only cleans Category A "Safe to Clean" items / 只清理 A 类「可安全清理」项目
- One-click operation, no risk of accidental deletion / 一键操作，无风险

**Mode 2: Custom Clean / 模式二：自定义清理**
- User selects which items to clean / 用户自由选择要清理的项目
- Re-confirm when Category B items are involved / 涉及 B 类项目时再次确认

Default recommendation: Quick Clean / 默认推荐「快速清理」。

---

### Step 5: Execute Cleanup / 第五步：执行清理

Clean each confirmed item sequentially. Report progress after each item.

按用户确认的项目逐项清理。每项清理后汇报进度。

#### Cleanup Commands / 清理命令

**1. User Temp Files / 用户临时文件：**
```powershell
Get-ChildItem $env:TEMP -ErrorAction SilentlyContinue | ForEach-Object {
    try { Remove-Item $_.FullName -Recurse -Force -ErrorAction Stop } catch {}
}
```

**2. Windows Temp Files / Windows 临时文件：**
```powershell
Get-ChildItem "C:\Windows\Temp" -ErrorAction SilentlyContinue | ForEach-Object {
    try { Remove-Item $_.FullName -Recurse -Force -ErrorAction Stop } catch {}
}
```

**3. Browser Cache (Edge/Chrome/Firefox) / 浏览器缓存：**
```powershell
# Delete all files in Cache directory
# Note: Some files may be locked if browser is running; skip is normal
Get-ChildItem $cachePath -Recurse -ErrorAction SilentlyContinue | Remove-Item -Force -Recurse -ErrorAction SilentlyContinue
```

**4. Thumbnail Cache / 缩略图缓存：**
```powershell
Get-ChildItem "$env:LOCALAPPDATA\Microsoft\Windows\Explorer" -Filter "thumbcache_*.db" -ErrorAction SilentlyContinue | Remove-Item -Force -ErrorAction SilentlyContinue
```

**5. npm Cache / npm 缓存：**
```powershell
npm cache clean --force
```

**6. Prefetch / 预读取文件：**
```powershell
Get-ChildItem "C:\Windows\Prefetch" -ErrorAction SilentlyContinue | Remove-Item -Force -ErrorAction SilentlyContinue
```

**7. Empty Recycle Bin / 清空回收站：**
```powershell
Clear-RecycleBin -Force -ErrorAction SilentlyContinue
```

**8. Specific Files in Downloads / 下载文件夹中的指定文件：**
- Only delete files the user explicitly names / 只删除用户明确点名的文件
- Confirm filename and size before deletion / 删除前再次确认文件名和大小
- Never batch-delete entire Downloads folder / 禁止批量删除整个下载文件夹

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

1. **Always scan before cleaning / 先扫描后清理** — Show results first, get confirmation before deleting / 先展示结果，得到确认再删除
2. **No unauthorized deletion / 不擅自做主** — Category B items require explicit user confirmation / B 类项目必须用户明确确认
3. **Skip in-use files / 跳过被占用文件** — Files in use can't be deleted, this is normal / 正在使用的文件删不掉是正常的
4. **Consistent size format / 统一大小格式** — Show MB for <1GB, GB for >=1GB, 2 decimal places / 小于 1GB 显示 MB，大于等于 1GB 显示 GB，保留 2 位小数
5. **Permission/sandbox limits / 权限与沙箱限制** — Inform user honestly, don't force operations / 如实告知用户，不要强行操作
6. **Never touch WinSxS / WinSxS 绝对不碰** — Manual deletion can break the system, recommend Disk Cleanup / 手动删除可能破坏系统，建议用磁盘清理工具
7. **No backup promises / 不做备份承诺** — These are temp/cache files, no backup needed, inform user / 清理的都是临时/缓存文件，不需要备份，提前告知用户
8. **Browser cache note / 浏览器缓存提醒** — Web pages may load slower once after cleanup (cache rebuilds), normal / 清理后网页首次加载可能稍慢（缓存重建），属正常现象
9. **npm cache note / npm 缓存提醒** — Next package install will re-download, may be slower / 下次安装包会重新下载，可能稍慢

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

- **Version / 版本**: 1.1.0
- **Author / 作者**: 29429
- **License / 许可证**: MIT
- **Repository / 仓库**: https://github.com/Zhu0512-ops/c-disk-cleaner
