# PCIe 课程笔记方法（MindShare 23 模块）

**视频位置**: `D:\master courses\560\PCIe\`  
**笔记仓库**: 本 repo 的 `notes/`（Obsidian vault 根目录）  
**计划对照**: `PCIe_project/01_PCIe_and_OS_Study_Plan.md` 每模块 checklist

---

## 每模块 5 步（约 3–4 小时）

| 步 | 做什么 | 笔记里写什么 |
|----|--------|--------------|
| 1 | 1× 速完整看一遍 | 只填 **Pass 1 大纲** + 时间戳 |
| 2 | 写 md（本文件） | 概念、对比表、ASCII 图 |
| 3 | 1.5× 重看难点 | 补 **Pass 2**、填 Questions |
| 4 | 对照 PCILeech（若适用） | 填 Cross-reference |
| 5 | 写 blog 草稿 | 填 Blog outline（`blogs/` 另开文件） |

**不要**逐字抄幻灯片。只写：定义、对比、因果、与后续模块/研究的链接。

---

## 新建一个模块笔记

```powershell
# 在 pcie-deep-dive 根目录
copy notes\_template_module.md notes\M01_background.md
```

命名规则：`M{编号}_{主题}.md`  
例：`M01_background.md`、`M02a_overview.md`、`M05_tlp_elements.md`

---

## 笔记结构（6 块）

1. **Metadata** — 模块号、视频文件名、日期、耗时  
2. **One-liner** — 用一句话说清本集核心  
3. **Pass 1 outline** — 看第一遍时按章节/时间戳列标题  
4. **Key concepts** — 按 `01_PCIe_and_OS_Study_Plan.md` 里该模块的 Notes 条目展开  
5. **Diagrams** — ASCII 或 `diagrams/M##_*.svg` 链接  
6. **Cross-reference** — PCILeech 文件、Thunderclap、后续模块 `[[wikilink]]`

---

## Obsidian 用法

- 用 `[[M03_config_space]]` 链到相关模块  
- Tag 建议：`#pcie/module01` `#pcileech` `#todo/rewatch`  
- 每日计划里的 `Write notes/M01_background.md` = 在本 vault 编辑后 `git push`

---

## 与论文笔记的区别

| | 论文 (Thunderclap) | PCIe 模块 |
|--|-------------------|-----------|
| 第一遍 | Abstract + 标题地图 | 整集 + 时间戳大纲 |
| 核心产出 | Threat model、Gap | 协议概念 + 拓扑/TLP 图 |
| 研究链接 | 直接进 arXiv 段落 | 经 Cross-ref 连到 PCILeech / 检测项目 |

论文摘要放在 `PCIe_project/papers/`；模块笔记放在本 repo `notes/`。
