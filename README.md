# Bright-Book-Breakdown

[English](README_EN.md) | **中文**

叙事体拆书工作流：从精读一本书到输出读书笔记 + 概念网络。

## 这是什么

一个面向 Claude Code 的拆书 skill，帮你把一本书拆解成**可导航的知识网络**，而不是简单的摘录笔记。

核心方法是**三轮认知压缩** + **双层结构笔记** + **wikilink 概念网络**，结合 **Obsidian** 使用效果最佳。

## Obsidian 联动

本 skill 专为 Obsidian 设计，输出可直接导入 Obsidian vault：

- **读书笔记** → `books/sources/`（叙事体 + 分析层）
- **概念页** → `books/concepts/`（可导航的知识节点）
- **Wikilink** → 连接成概念网络，双向可跳转

Obsidian 的双向链接功能让概念页之间形成网状结构，点击任意 `[[wikilink]]` 即可跳转，层层深入。

## 适用场景

- 你读了一本非虚构书，想建自己的知识库
- 你想把书里的概念和其他知识连接起来
- 你用的是 Obsidian 或类似工具

## 工作流全貌

```
接收书籍
  │
  ├── Step 1: 三轮认知压缩（内部分析，不输出）
  │    ├── 骨架扫描 → 全书在说什么
  │    ├── 血肉解剖 → 凭什么这么说
  │    └── 灵魂提取 → 还能怎么用
  │
  ├── Step 2: 写叙事体读书笔记
  │    ├── 叙事层：章节叙述（400-800字/章）
  │    └── 分析层：批判分析组件
  │
  └── Step 3: 验证并更新关联概念
       └── 首次出现即 [[wikilink]]
```

### 批量拆书（Batch Ingest）

适合一次性拆完全书，生成概念网络：

```
PDF/EPUB
  │
  ├── 文本提取
  │    ├── 优先：PyMuPDF/pdfplumber（文本层）
  │    └── 降级：PaddleOCR（扫描 PDF）
  │
  ├── 并行 Agent 拆解
  │    └── 每章 → 3-8 个概念页
  │
  └── Wikilink 验证
       └── 悬空链接 → 先建概念页再补
```

## 文本层提取规则

| 情况 | 工具 | 说明 |
|------|------|------|
| PDF 有嵌入文本 | PyMuPDF / pdfplumber | 直接提取，速度快准确率高 |
| 文本乱码/乱序 | pdfplumber 备选 | 换工具重试 |
| 扫描 PDF（无文本层）| PaddleOCR GPU | 逐页渲染 + OCR 识别 |

**降级顺序**：PyMuPDF → pdfplumber → OCR

OCR 仅作为最后手段，不要默认使用 OCR。

## 输出结构

```
vault/
├── books/
│   ├── sources/        # 读书笔记（叙事体 + 分析层）
│   ├── concepts/       # 概念页网络
│   ├── entities/       # 人物/组织实体页
│   └── meta/
│       ├── index.md    # 知识库索引
│       └── log.md      # 操作日志
```

### 概念页格式

```markdown
---
title: {概念名称}
type: concept
created: {日期}
updated: {日期}
tags: [标签1, 标签2]
sources: [来源文件路径]
---

# {概念名称}

> 一句话定义

## 核心内容
（从原文中提炼的关键要点）

## 操作指引
（这个概念如何落地到实际工作中）

## 关联概念
- [[相关概念1]]
- [[相关概念2]]

## 参考
- [[来源页面]]
```

### Wikilink 规范

- **首次出现**：在正文中加 `[[wikilink]]`
- **再次出现**：纯文本，不加链接
- **关联概念节**：底部按主题分组列出

**验证**：任何 `[[wikilink]]` 指向的文件必须先存在，不允许悬空链接。

## 使用方法

### 通过 Claude Code 调用

```
/bright-book-breakdown 《书名》+{具体需求}
```

示例：
```
/bright-book-breakdown 《思考，快与慢》
/bright-book-breakdown 《合同起草审查指南》+改写成叙事体
/拆书 《企业合规指南》+Batch Ingest
```

### 集成到自己的 skills

1. 下载 `SKILL.md`
2. 放入 Claude Code skills 目录
3. 重启 Claude Code

## 三轮认知压缩

| 轮次 | 目标 | 回答的问题 |
|------|------|-----------|
| 骨架扫描 | 建立全局结构 | "这本书在说什么" |
| 血肉解剖 | 理解论证链条 | "凭什么这么说" |
| 灵魂提取 | 超越作者 | "还能怎么用" |

## 与其他方法的区别

| 方法 | 特点 | 区别 |
|------|------|------|
| 抄书笔记 | 摘录段落 | 本 skill 不抄段落，只提取结构 |
| 思维导图 | 树状发散 | 本 skill 有叙事逻辑 + 批判分析 |
| 卡片盒 | 原子化卡片 | 本 skill 强调首次出现即链接，形成网络 |

## 依赖环境

- [Claude Code](https://claude.ai/code)
- Obsidian（可选，用于概念网络）
- PyMuPDF / pdfplumber（文本提取）
- PaddleOCR（扫描 PDF 降级使用）

## 文件结构

```
bright-book-breakdown/
├── SKILL.md          # 技能定义文件（核心）
├── README.md         # 中文说明
├── README_EN.md     # English README
└── examples/        # 示例文件
    ├── INDEX.md     # 示例说明
    ├── 精益生产.md  # 概念页格式示例
    └── 失去的制造业.md  # 读书笔记格式示例
```

## 示例说明

两个完整示例，展示从精读到概念页的完整流程：

| 文件 | 类型 | 说明 |
|------|------|------|
| [examples/失去的制造业.md](examples/失去的制造业.md) | 读书笔记 | 叙事层 + 分析层 |
| [examples/精益生产.md](examples/精益生产.md) | 概念页 | 标准格式，可直接复用 |

## 参考与致谢

站在前辈们的肩上，本 skill 借鉴了以下开源项目：

- [LLM Wiki](https://github.com/karpathy/llm-utils) by @karpathy — LLM 与知识库的结合
- [Im-wiki-obsidian-blink](https://github.com/iBlinkQ/Im-wiki-obsidian-blink) by @iBlinkQ — Obsidian + Claude Code 联动
- [claude-obsidian](https://github.com/AgriciDaniel/claude-obsidian) by @AgriciDaniel — Claude 与 Obsidian 的集成思路

## License

MIT