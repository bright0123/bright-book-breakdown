# Bright Book Breakdown

叙事体拆书工作流 — 一个 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) Skill，核心是三轮认知压缩 + 双层结构笔记 + 首次出现即链接的 wikilink 规范。

## 功能

- **输入自由**：不拘泥于 PDF 或 EPUB，支持绝大多数书籍格式
  - 条件：书籍内容需转化为 Claude Code Skill 能读取的文本文件（.txt、.md）
  - 例如：PDF → 用 OCR 提取为 .txt；EPUB/azw3/mobi → 用解析工具转 .md；扫描书 → 拍照后 OCR
- 三轮认知压缩：骨架扫描 → 血肉解剖 → 灵魂提取
- 输出叙事体读书笔记：可精读的章节叙述 + 批判分析层
- 首次出现即链接的 wikilink 规范：叙事层中跳转，底部关联概念提供全貌
- **可选：批量入知识库** — 配合 Obsidian vault，将全书拆解为互联概念网络

## 安装

将 skills 目录放在 Claude Code 的 skills 目录下。

## 使用

```
/bright-book-breakdown 《书名》
/拆书 《书名》
/拆书 《书名} + {具体需求}
```

## 工作流两层架构

### 第一层：单本精读（Step 1-3）

```
接收书籍
  │
  ├── Step 1：三轮认知压缩（内部分析，不输出）
  │    ├── 骨架扫描 → 全局结构
  │    ├── 血肉解剖 → 论证链条
  │    └── 灵魂提取 → 超越作者
  │
  ├── Step 2：写叙事体读书笔记（输出到 books/sources/）
  │    ├── 叙事层：可精读的章节叙述
  │    └── 分析层：批判分析组件
  │
  └── Step 3：验证并更新关联概念（底部按主题分组）
```

### 第二层：批量入知识库（可选，Batch Ingest）

配合 Obsidian vault 使用，将全书拆解为互联概念网络。**一次性动作，不是持续建设。**

```
Batch Ingest 一本书
    │
    ├── Phase 1：文本提取（优先文本层，OCR 降级）
    │    ├── 文本层可用（PDF/EPUB 内嵌文本）→ 直接提取
    │    └── 扫描版 / 文本层损坏 → OCR 提取为 .txt
    │
    ├── Phase 2：章节分割
    │    ├── 按目录分章节输出为独立 .txt 文件
    │    └── 存储于 vault 的 raw/chapters/ 目录
    │
    ├── Phase 3：并行 Agent 拆解
    │    ├── 每章节 → 一个 background agent
    │    ├── 每个 agent 识别 3-8 个子概念
    │    ├── 每个子概念创建 books/concepts/ 页面
    │    └── 用 [[wikilink]] 链接到相关概念
    │
    └── Phase 4：整合
         ├── Wikilink 验证（扫描所有新页面，悬空链接先建概念页再补链）
         ├── 更新 books/meta/index.md（全局索引）
         ├── 追加 books/meta/log.md（操作记录）
         └── 更新 books/meta/hot.md（高价值页面标记）
```

## Vault 输出结构

运行 Batch Ingest 后，vault 目录结构如下：

```
vault/
├── raw/                          # 原始书籍文件（一次性存入）
│   ├── books/                     # 原始书籍（PDF/EPUB/TXT 等）
│   └── chapters/                  # 按章节分割后的文本
├── books/
│   ├── sources/                  # 读书笔记（叙事体，可精读）
│   │   └── {书名}.md
│   ├── concepts/                 # 子概念页（可批量生成）
│   │   └── {概念名}.md
│   ├── entities/                 # 人物/组织/书籍等实体
│   ├── comparisons/              # 比较分析笔记
│   ├── meta/
│   │   ├── index.md              # 全局索引（所有概念页的目录）
│   │   ├── log.md                # 操作记录（每次拆书追加）
│   │   └── hot.md                # 高价值页面标记（按热度排序）
│   └── sources/                  # 各分类书籍目录
└── wiki/                         # 主题综述笔记（可选）
```

**说明**：
- `meta/index.md`、`meta/log.md`、`meta/hot.md` 是知识库的导航基础设施
- `raw/` 存放原始书籍文件，拆完后可清空或保留
- 概念页生成后通过 [[wikilink]] 互相连接，形成可导航的知识图谱

## Wikilink 规范

**首次出现即链接**：概念第一次在正文中出现时 → inline `[[wikilink]]`；同一概念再次出现 → 不加链接。

**前提条件**：概念文件必须先存在。Batch Ingest 模式下由 Phase 3 的 Agent 预先创建。

## 致谢

本 skill 借鉴了以下项目/文章的思想：

- **Karpathy's LLM Wiki** — LLM 作为本地知识库的核心思路
- [claude-obsidian](https://github.com/AgriciDaniel/claude-obsidian) — Claude Code 与 Obsidian 联动的工作流启发
- [Im-wiki-obsidian-blink](https://github.com/iBlinkQ/Im-wiki-obsidian-blink) — Obsidian 双链笔记与 AI 结合的实践参考

## 版权

MIT License