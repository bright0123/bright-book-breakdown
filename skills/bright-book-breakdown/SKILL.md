---
name: bright-book-breakdown
description: 拆书工作流：从精读一本书到输出叙事体读书笔记 + 概念网络。核心是三轮认知压缩 + 双层结构笔记 + 首次出现即链接的 wikilink 规范。
user_invocable: true
---

# Bright-Book-Breakdown：叙事体拆书工作流

> **适用范围**：本 skill 仅适用于"书"（教材、专著、方法论、非虚构类书籍）。
> 不适用于案例材料、合同文本、判决书、法律意见书等法律文档。

## 核心理念

读书不是抄笔记，是**提取结构 + 建立连接 + 产出行动**。

- **提取结构**：通过三轮认知压缩，把一本书的骨架、血肉、灵魂全部提取出来
- **建立连接**：通过 wikilink 把新知识与已有知识网络连接，形成可导航的知识图谱
- **产出行动**：读书后必须知道"应该做什么不同的事"，否则等于没读

---

## 全流程概览

```
接收书籍
  │
  ├── Step 1: 三轮认知压缩（内部分析，不输出）
  │    ├── 骨架扫描 → 全局结构
  │    ├── 血肉解剖 → 论证链条
  │    └── 灵魂提取 → 超越作者
  │
  ├── Step 2: 写叙事体读书笔记（输出到 books/sources/）
  │    ├── 叙事层：可精读的章节叙述（400-800字/章）
  │    └── 分析层：批判分析组件（核心问题→关联概念）
  │
  └── Step 3: 验证并更新关联概念（底部按主题分组）
```

---

## Step 1：三轮认知压缩

内部分析框架，为 Step 2 的输出提供原材料。

### 第一轮：骨架扫描 (Skeleton Scan)

目标：建立全局结构，回答"这本书在说什么"

- **核心问题**：作者试图回答什么问题？
- **核心答案**：作者的回答是什么？（一句话）
- **章节骨架**：每章的核心论点（不超过 10 字/章）
- **论证结构**：演绎型、归纳型、案例型、对比型？
- **逻辑主线**：从起点到结论的推理路径

### 第二轮：血肉解剖 (Deep Dissection)

目标：理解论证链条，回答"凭什么这么说"

- **核心论证链**：作者用什么逻辑支撑核心答案？
- **关键证据**：最有说服力的 3 个证据/案例
- **隐形假设**：作者没说但必须成立的前提
- **反例与边界**：在什么情况下作者的结论会失效？

### 第三轮：灵魂提取 (Soul Extraction)

目标：超越作者，回答"还能怎么用"

- **作者盲点**：作者没看到什么？
- **可迁移模式**：这个思想在其他领域叫什么？
- **与我的连接**：这本书与已有知识的交叉点
- **行动触发**：读完这本书，应该做什么不同的事？

---

## Step 2：输出叙事体读书笔记

### 模板结构（双层：叙事层 + 分析层）

```markdown
---
title: {书名}
type: source
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
tags: [标签1, 标签2]
sources: []
---

# {书名}

- **作者**：{作者}
- **类型**：书籍
- **版本**：{版本}
- **状态**：精读完成

---

## 导论：{一句话核心命题}

{300-500 字：说明全书在回答什么问题，作者的核心答案是什么，
全书的主线是什么，使用说明（适合精读/可按篇查阅）}

---

## 第一篇：{篇名}

### Ch1 {章节名}

{400-800 字：讲什么、核心论点、为什么重要}
{此章首次出现 [[概念A]]、[[概念B]] 时，inline 加链接}

### Ch2 {章节名}

{400-800 字}

---

## 第二篇：{篇名}

### Ch3 {章节名}

{400-800 字；首次出现 [[概念C]] 时 inline 加链接}

---

## 附论：工具箱

### 审查清单
{勾选清单}

### 行动触发
{3-5 条读完应该做什么}

---

## 分析层

### 核心问题
> {作者试图回答什么问题}

### 核心答案
> {一句话概括作者的回答}

### 论证链
```
{前提1} → {前提2} → ... → {结论}
```

### 关键证据
1. {最有说服力的证据/案例}
2. {证据2}
3. {证据3}

### 隐形假设
- {作者没说但必须成立的前提}

### 边界条件
- {在什么情况下结论会失效}

### 作者盲点
{作者没看到什么}

---

## 关联概念

### 核心理念
- [[概念A]] — 书中论述的核心原理
- [[概念B]] — 支撑核心命题的基础理论

### [主题分类1]
- [[概念C]]
- [[概念D]]

### 跨域连接
- [[概念E]]（来源：[[相关领域读书笔记]]）
```

### Wikilink 规范（核心规则）

**首次出现即链接**：
- 概念第一次在正文中出现时 → inline `[[wikilink]]`，可跳转到概念页
- 同一概念在正文中再次出现 → 不加链接，纯文本
- "关联概念"节（底部）→ 按主题分组列出所有相关概念

**为什么这样设计**：
- 叙事层中加链接 → 方便读者精读时随时跳转深入
- 底部关联概念 → 提供全貌视角，按主题检索而非罗列

**前提条件：概念文件必须先存在**

在叙事层写入任何 `[[wikilink]]` 之前，必须先用验证脚本确认目标文件存在：

```bash
grep -o '\[\[[^]]*\]\]' books/sources/{书名}.md | grep -v '\[\[wikilink\]\]' | sed 's/\[\[//;s/\]\]//' | sort -u | while read link; do
  if [ ! -f "books/concepts/${link}.md" ] && [ ! -f "books/entities/${link}.md" ] && [ ! -f "books/sources/${link}.md" ]; then
    echo "MISSING: $link"
  fi
done
```

如果输出 `MISSING: xxx`，**先创建** `books/concepts/xxx.md`（填入一句话定义和核心内容），然后再在叙事层使用 `[[xxx]]`。不允许存在悬空链接。

**绝对禁止**：
- `[[wikilink]]` 作为示例占位符保留在正文里（应直接删除该句）
- 链接指向不存在的文件

### 叙事层写作规范

- **段落叙述为主**：每个章节 400-800 字，用完整的段落讲述，不堆砌要点
- **章首一句回核心**：每章开头用一句话回到全书核心命题，保持主线连贯
- **自然嵌入 wikilink**：首次提到某概念时自然嵌入 `[[wikilink]]`，不加刻意
- **避免过短或过碎**：核心章节可至 1000 字，不要为了分段而破坏叙事连贯

### 分析层写作规范

- **核心问题**：作者试图回答什么问题（不是书的标题问题）
- **核心答案**：一句话概括作者的回答
- **论证链**：用 ASCII art 树状图表示，不超过 6 行
- **行动触发**：必须产出可执行的行动，不是"要重视"之类的废话

### 写入规范

1. 文件名：`books/sources/{书名}.md`，书名不用《》
2. 标签至少包含：`book`、`精读`
3. 叙事层章节数根据原书结构调整（4-10 章均可）
4. 分析层各组件必须完整，不可省略

---

## Step 3：关联概念写入规范

### 提取方法

**从三个来源扫描：**

1. **正文各章节** — 章节叙述中提到的方法论、原则、工具
2. **分析层各组件** — 核心问题、论证链、关键证据、行动触发
3. **读书笔记中实际出现的概念名** — 不是猜测，而是书中真实提到的

**扫描命令：**
```bash
grep -o '\[\[[^]]*\]\]' books/sources/{书名}.md | grep -v '\[\[wikilink\]\]' | sed 's/\[\[//;s/\]\]//' | sort -u
```

### 判断标准

```
这个概念在书中是否真实出现或被引用？
├── 否 → 不写入
└── 是 → 它属于哪个主题/章节？
    ├── 属于本书主题范围 → 按主题分组写入
    └── 只被提及一次，无深度展开 → 写入"其他相关"区
```

### 质量标准

1. **链接必须存在** — 写入前逐个验证文件存在（concepts/ 或 entities/ 或 sources/）
2. **分组要清晰** — 按主题或章节分组，同组概念逻辑相近
3. **覆盖要全面** — 不遗漏书中实际提到的每个概念
4. **命名要准确** — 必须与实际文件名完全一致
5. **无悬空链接** — 不得有任何 `[[wikilink]]` 指向不存在的文件

### 概念双向链接规范

**概念之间也要互相连接**——每个 concept 页的"关联概念"节应包含相关概念的 `[[wikilink]]`，不是单向，而是双向呼应。

例如：若 `合同缺陷三层次.md` 的关联概念中写了 `[[致命缺陷]]`，则 `致命缺陷.md` 的关联概念中也应包含 `[[合同缺陷三层次]]`。

**操作步骤**：
1. 创建或编辑 concept 页时，先列出所有相关概念
2. 对每个相关概念，验证目标文件存在
3. 将相关概念写入"关联概念"节（用 `[[wikilink]]`）
4. 检查目标 concept 页是否已有反向链接，若无则补上

### 验证脚本

```bash
# 检查 sources 中的 wikilink
for book in "书名"; do
  grep -o '\[\[[^]]*\]\]' "books/sources/${book}.md" | grep -v '\[\[wikilink\]\]' | sed 's/\[\[//;s/\]\]//' | sort -u | while read link; do
    if [ ! -f "books/concepts/${link}.md" ] && [ ! -f "books/entities/${link}.md" ] && [ ! -f "books/sources/${link}.md" ]; then
      echo "MISSING: $link"
    fi
  done
done

# 检查 concepts 中的 wikilink（双向链接验证）
for f in books/concepts/*.md; do
  grep -o '\[\[[^]]*\]\]' "$f" | sed 's/\[\[//;s/\]\]//' | while read link; do
    if [ ! -f "books/concepts/${link}.md" ] && [ ! -f "books/entities/${link}.md" ] && [ ! -f "books/sources/${link}.md" ]; then
      echo "DEAD: $(basename $f) -> $link"
    fi
  done
done
```

### 分组原则

- 每组 3-8 个概念为宜；超过 8 个需拆分
- 组标题使用书中的核心主题词
- 最后一个分组为"跨域连接"，放引用了外书的概念

---

## 额外可选阶段：Batch Ingest（批量拆书入知识库）

适用于需要将全书拆解为互联概念网络的场景。**需要配合 Obsidian vault 使用。**

### 核心原则：一次性建设完成

概念页的批量生成是**一次性动作**——不是持续建设。

批量拆书时，概念页应当一次性生成完毕（并行 Agent 一次性拆完全部章节），而不是逐章渐进建设。

### 前置条件：文本提取（文本层优先，OCR 降级）

**优先使用文本层** — PDF/EPUB 如果有内嵌文本层，优先用 PyMuPDF 或 pdfplumber 直接提取，速度快且准确率高。

**降级顺序**：
1. **PyMuPDF** (`fitz`)：首选，直接提取嵌入文本
2. **pdfplumber**：备选，若 PyMuPDF 提取的文本有乱码/乱序，用 pdfplumber 重试
3. **OCR**：最后手段，仅当文本层完全不可用（扫描 PDF）时才使用 PaddleOCR

**文本层检测脚本**：
```python
import fitz
doc = fitz.open('path/to/file.pdf')
page = doc[sample_page]
text = page.get_text()
has_text = len(text.strip()) > 100
is_garbled = '�' in text or '丶' in text
if has_text and not is_garbled:
    print("文本层可用，直接提取")
else:
    print("降级至 pdfplumber 或 OCR")
```

**pdfplumber 提取**：
```python
import pdfplumber

with pdfplumber.open("path/to/file.pdf") as pdf:
    full_text = ""
    for page in pdf.pages:
        text = page.extract_text()
        if text:
            full_text += text + "\n"
```

**OCR 环境**（PaddleOCR，GPU mode）：
```python
import os
os.environ['CUDA_VISIBLE_DEVICES'] = '0'
import paddle
paddle.set_device('gpu')

from paddleocr import PaddleOCR
ocr = PaddleOCR(
    lang='ch',
    use_textline_orientation=False,
    use_doc_orientation_classify=False,
    use_doc_unwarping=False,
    text_detection_model_name='PP-OCRv5_server_det',
    text_recognition_model_name='PP-OCRv5_server_rec',
)

import fitz
doc = fitz.open('path/to/book.pdf')
page = doc[0]
# 分辨率用 1x，2x/3x 会导致 GPU 显存不足（CUDA OOM 或 CUDNN 错误）
pix = page.get_pixmap(matrix=fitz.Matrix(1.0, 1.0))
pix.save('/tmp/page.png')
result = ocr.ocr('/tmp/page.png')
# PaddleOCR 3.5+ 返回 OCRResult 对象列表
# 文本从 result[0].str['res']['rec_texts'] 取
texts = result[0].str['res']['rec_texts']
full_text = '\n'.join(texts)
```

### Batch Ingest 工作流

```
Batch Ingest 多本书（并行）
    │
    ├── Phase 1: 文本提取（多书并行）
    │    ├── 用 PyMuPDF/pdfplumber 直接提取各书文本
    │    ├── 验证：抽样3页/书，检查提取质量
    │    └── 若文本层正常 → 直接使用；异常则换 pdfplumber 再试
    │
    ├── Phase 2: 章节分割
    │    ├── 根据目录确定各单元页码范围
    │    └── 分片存储，每章/单元一个文件
    │
    ├── Phase 3: 并行 Agent 拆解（核心）
    │    ├── 每本书 → 一个 background agent
    │    ├── 每 agent 内部：每章/单元 → sub-agent 并行
    │    ├── Agent 接收：文本 + vault 路径 + 已有页面列表 + 读书笔记
    │    ├── Agent 工作：
    │    │    ├── 识别 3-8 个子概念
    │    │    ├── 每个子概念创建 books/concepts/ 页面
    │    │    └── 用 [[wikilink]] 链接到同级和上级概念
    │    └── 用 Agent tool + run_in_background: true 并行运行（4书同时）
    │
    └── Phase 4: 整合
         ├── 收集所有新页面
         ├── Wikilink 验证：全部新页面扫描，悬空链接先建概念页再补链接
         ├── 更新 books/meta/index.md
         ├── 追加 books/meta/log.md
         └── 更新 books/meta/hot.md
```

### Wikilink 验证流程（每次批量后必须执行）

**在读书笔记写入任何 [[wikilink]] 之前**，必须先验证所有链接目标存在：

```bash
grep -o '\[\[[^]]*\]\]' books/sources/{书名}.md | grep -v '\[\[wikilink\]\]' | sed 's/\[\[//;s/\]\]//' | sort -u | while read link; do
  if [ ! -f "books/concepts/${link}.md" ] && [ ! -f "books/entities/${link}.md" ] && [ ! -f "books/sources/${link}.md" ]; then
    echo "MISSING: $link"
  fi
done
```

若输出 `MISSING: xxx`：
1. **先创建** `books/concepts/xxx.md`（填入一句话定义 + 核心内容摘要）
2. 然后再在读书笔记叙事层使用 `[[xxx]]`

不允许悬空链接存在。

### Agent 拆解指令模板

处理每章时，发给 background agent 的标准指令。**必须先有读书笔记作为全局上下文。**

```
## 任务：从 {书名} 的章节文本中提取子概念，创建 Obsidian wiki 页面

### 上下文
本书读书笔记：{读书笔记文件路径}
vault 路径：d:\files\cc+obsidian

### 输入
{文本文件路径}

### 输出要求
为每个子概念创建 books/concepts/ 下的 markdown 文件。

每个页面格式：
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
- [[相关概念3]]

## 参考
- [[来源页面]]
- [[读书笔记页面]]
```

**双向链接要求**：写入 [[相关概念1]] 时，必须同时检查该 concept 页是否已反链回来。若无，在该页的关联概念节补上本概念的 `[[wikilink]]`。概念之间的关系应当是双向可导航的。

### Wiki 页面类型规范

| 类型 | 适用场景 |
|------|---------|
| concept | 核心概念、方法、理论、模型 |
| entity | 人物、组织、书籍、项目 |
| source | 来源摘要（读书笔记） |
| comparison | 比较分析笔记 |
| overview | 主题综述笔记 |

每个页面必须有 `[[wikilink]]` 指向相关页面。**操作指引**部分是必须的——说明这个概念怎么用，不能只有理论描述。

---

## 工作流总结

| 步骤 | 输入 | 输出 | 核心动作 |
|------|------|------|----------|
| Step 1 | 书的全文 |（内部分析，不落盘）| 三轮认知压缩 |
| Step 2 | 压缩结果 | `books/sources/{书名}.md` | 写叙事体笔记 + 首次出现即链接 |
| Step 3 | 读书笔记 | 底部关联概念节 | 扫描→分组→验证 |
| Batch Ingest（可选）| PDF/EPUB | concepts/ 概念页网络 | 文本层提取 + 并行 Agent 拆解 |

---

## 唤醒指令

用户可通过以下方式唤醒此技能：
- `/bright-book-breakdown 《书名》`
- `/拆书 {书名}`
- `/拆书 {书名} + {具体需求}`（如"要改写成叙事体"）