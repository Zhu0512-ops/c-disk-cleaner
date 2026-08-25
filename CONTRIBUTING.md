# Contributing to c-disk-cleaner

First off, thanks for taking the time to contribute! ❤️

All types of contributions are encouraged and valued. See the [Table of Contents](#table-of-contents) for different ways to help and details about how this project handles them.

## Table of Contents

- [I Have a Question](#i-have-a-question)
- [I Want To Contribute](#i-want-to-contribute)
  - [Reporting Bugs](#reporting-bugs)
  - [Suggesting Enhancements](#suggesting-enhancements)
  - [Improving Documentation](#improving-documentation)
  - [Submitting Code Changes](#submitting-code-changes)
- [Style Guide](#style-guide)
- [Commit Messages](#commit-messages)

---

## I Have a Question

If you want to ask a question, we assume that you have read the available [Documentation](README.md).

Before you ask a question, it is best to search for existing [Issues](https://github.com/Zhu0512-ops/c-disk-cleaner/issues) that might help you. In case you have found a suitable issue and still have questions, you can write your question in that issue. It is also advisable to search the internet for answers first.

If you then still feel the need to ask a question and need clarification, we recommend the following:

1. Open an [Issue](https://github.com/Zhu0512-ops/c-disk-cleaner/issues/new).
2. Provide as much context as you can about what you're running into.
3. Provide your environment details (Windows version, PowerShell version, TRAE version, etc.).

We will take care of the issue as soon as possible.

---

## I Want To Contribute

> ### Legal Notice
> When contributing to this project, you must agree that you have authored 100% of the content, that you have the necessary rights to the content, and that the content you contribute may be provided under the project license.

### Reporting Bugs

#### Before Submitting a Bug Report

A good bug report shouldn't leave others needing to chase you up for more information. Therefore, we ask you to investigate carefully, collect information, and describe the issue in detail in your report.

**Make sure the following is included:**

- **Steps to reproduce**: Clearly list the exact steps that lead to the issue.
- **Expected behavior**: What you expected to happen.
- **Actual behavior**: What actually happened.
- **Environment details**:
  - Windows version (e.g., Windows 11 22H2)
  - PowerShell version (`$PSVersionTable.PSVersion`)
  - TRAE Work version
  - Skill version
- **Screenshots / logs**: If applicable, add screenshots or error logs.

#### How to Submit a Bug Report

1. Go to [Issues](https://github.com/Zhu0512-ops/c-disk-cleaner/issues).
2. Use the **Bug Report** template.
3. Fill in all the requested information.
4. Submit the issue.

### Suggesting Enhancements

This section guides you through submitting an enhancement suggestion for c-disk-cleaner, **including completely new features and minor improvements to existing functionality**.

#### Before Submitting an Enhancement

- Make sure you are using the latest version.
- Read the [documentation](README.md) carefully and check if the feature already exists.
- Perform a [search](https://github.com/Zhu0512-ops/c-disk-cleaner/issues) to see if the enhancement has already been suggested. If it has, add a comment to the existing issue instead of opening a new one.
- Find out whether your idea fits with the scope and aims of the project.

#### How to Submit a Feature Request

1. Go to [Issues](https://github.com/Zhu0512-ops/c-disk-cleaner/issues).
2. Use the **Feature Request** template.
3. Clearly describe the feature and its benefits.
4. Submit the issue.

### Improving Documentation

Documentation improvements are always welcome! Whether it's fixing typos, adding examples, or clarifying confusing sections — every bit helps.

**Types of documentation contributions:**
- README.md improvements
- SKILL.md clarifications
- Adding translation support
- Adding usage examples
- Fixing typos and grammar

### Submitting Code Changes

1. Fork the repository.
2. Create a new branch from `main` (e.g., `feature/new-scan-category` or `fix/temp-file-bug`).
3. Make your changes.
4. Test thoroughly on a Windows system.
5. Commit your changes following the [commit message conventions](#commit-messages).
6. Open a Pull Request.

#### Pull Request Checklist

- [ ] My code follows the style guidelines of this project.
- [ ] I have performed a self-review of my own code.
- [ ] I have commented my code, particularly in hard-to-understand areas.
- [ ] I have made corresponding changes to the documentation.
- [ ] My changes generate no new warnings.
- [ ] I have tested my changes on a Windows system.
- [ ] The skill still works correctly after my changes.

---

## Style Guide

### SKILL.md Conventions

- Use YAML frontmatter with `name` and `description` fields.
- The `description` must include both what the skill does AND when to invoke it.
- Keep the description under 200 characters.
- Use bilingual (English + Chinese) structure for all content sections.
- Code examples use PowerShell syntax highlighting.
- PowerShell commands should include error handling (`-ErrorAction SilentlyContinue`, try/catch).

### Code Style

- Use PowerShell 5.0 compatible syntax (not PowerShell 7 exclusive features).
- Include proper error handling for all file operations.
- Use `$env:` variables instead of hardcoded paths.
- Format sizes with consistent units (MB for < 1GB, GB for >= 1GB, 2 decimal places).

---

## Commit Messages

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Types

- `feat`: New feature / 新功能
- `fix`: Bug fix / Bug 修复
- `docs`: Documentation changes / 文档变更
- `style`: Code style changes (formatting, etc.) / 代码风格变更
- `refactor`: Code refactoring / 代码重构
- `perf`: Performance improvement / 性能优化
- `test`: Adding tests / 添加测试
- `chore`: Maintenance tasks / 维护任务

### Examples

```
feat: add Windows Update cache scanning
fix: handle null size in folder size calculation
docs: update README with bilingual content
refactor: extract helper functions to reduce duplication
```

---

## Join The Project Team

If you're interested in becoming a regular contributor or maintainer, reach out by opening an issue with the title "Interested in joining as a contributor".

---

## Attribution

This guide is based on the **contributing-gen** and open source community standards.

