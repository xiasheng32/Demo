---
name: pdf-literature
description: 从PDF文献中提取关键实验信息。当有pdf文件输入时，判断是否是文献，是文献则开始执行流程；不是则向我询问接下来的操作。
---

## 执行流程

### Step 1：定位PDF文件

若用户提供的路径不存在，在Zotero存储中模糊查找：

```bash
python3 -c "
import glob, sys
ZOTERO = '/Users/xias/Zotero/storage'
query = sys.argv[1]
results = glob.glob(f'**/*{query}*.pdf', root_dir=ZOTERO, recursive=True)
for r in results: print(f'{ZOTERO}/{r}')
"
```

若找到多个匹配，使用 `question` 工具让用户选择。

### Step 2：智能提取关键段落

**核心优化**：在Python端完成文本切割，只输出模型需要的3个部分，跳过无关内容。

```bash
python3 << 'PYEOF'
import sys

pdf_path = sys.argv[1]

try:
    import fitz
except ImportError:
    fitz_available = False
else:
    fitz_available = True

if fitz_available:
    try:
        doc = fitz.open(pdf_path)
    except Exception:
        import pdfplumber
        with pdfplumber.open(pdf_path) as pdf:
            pages = [p.extract_text() or "" for p in pdf.pages]
    else:
        pages = [p.get_text() for p in doc]
        doc.close()
else:
    import pdfplumber
    with pdfplumber.open(pdf_path) as pdf:
        pages = [p.extract_text() or "" for p in pdf.pages]

# === 部分1：首页（题目、作者、摘要）===
print("=== PART1: TITLE_PAGE ===")
print(pages[0] if pages else "")

# === 部分2：Materials and Methods 章节 ===
full = "\n".join(pages)
import re
method_patterns = [
    r"(?:Materials\s+and\s+|Experimental\s+)?(?:M|m)aterials?\s+and\s+(?:M|m)ethods?",
    r"(?:M|m)ethods?\s*(?:and\s+Materials?)?",
]
method_start = None
for pat in method_patterns:
    m = re.search(pat, full)
    if m:
        method_start = m.start()
        break

if method_start is not None:
    end_patterns = [r"\nResults\b", r"\nResults\s+and\s+Discussion", r"\nDiscussion\b"]
    method_end = len(full)
    for ep in end_patterns:
        em = re.search(ep, full[method_start + 100:])
        if em:
            method_end = method_start + 100 + em.start()
            break
    method_text = full[method_start:method_end]
    if len(method_text) > 6000:
        method_text = method_text[:6000] + "\n...[truncated]"
    print("\n=== PART2: METHODS ===")
    print(method_text)
else:
    print("\n=== PART2: METHODS (fallback: early pages) ===")
    for p in pages[1:3]:
        print(p[:2000])

# === 部分3：图表描述 ===
fig_table_lines = []
for i, page in enumerate(pages):
    for line in page.split("\n"):
        stripped = line.strip()
        if re.match(r'^(?:Fig\.?|Figure|Table|图|表)\s*\d', stripped, re.IGNORECASE):
            idx = page.index(stripped)
            snippet = page[idx:idx+300]
            fig_table_lines.append(snippet.rstrip())

if fig_table_lines:
    print("\n=== PART3: FIGURES_TABLES ===")
    for ft in fig_table_lines:
        print(ft)
        print("---")

PYEOF
```

将上面脚本的输出直接用于后续分析，无需写入临时文件再读回。

### Step 3：信息提取

从输出的3个部分中提取信息：

**PART1 → 基本信息 + 作物名称**
- 题目、作者在首页顶部
- 摘要中通常包含作物名称

**PART2 → 实验信息**
- 生长条件、生长时期、胁迫方式、浓度、持续时间、重复数

**PART3 → 图表清单**
- 直接列出图号和描述

---

## 最终输出模板

**⚠️ 强制要求（单篇和批量均适用）：输出时必须严格按照下方模板，逐字逐句复制使用，不得增加、删除、修改任何字段或表结构。如有字段文中未提及，填"文中未明确提及"，不得自行推测或留空。批量处理时，每篇PDF各自独立输出一遍完整模板，不得合并、省略、简化或只输出差异部分。每篇的输出之间用 `---` 分隔。**

```markdown
## 基本信息

| 信息项 | 提取结果 |
|--------|----------|
| 文献题目 | （翻译为中文） |
| 作者 | |

## 步骤1：信息匹配

| 信息项 | 提取结果 |
|--------|----------|
| 作物名称 | |
| 作物科属 | |
| 生长条件 | |
| 生长时期 | |
| 干旱胁迫模拟方式 | |
| 胁迫浓度 | |
| 胁迫持续时间 | |
| 重复数 | |

## 步骤2：图表清单

### 图片
| 图号 | 内容 |
|------|------|
| Fig. 1 | |

### 表格
| 表号 | 内容 |
|------|------|
| Table 1 | |
```

---

## 异常处理

| 异常场景 | 处理方式 |
|----------|----------|
| PDF路径不存在 | 使用glob在Zotero存储中模糊查找 |
| 找到多个匹配PDF | 使用question工具让用户选择 |
| PyMuPDF不可用 | 自动回退到pdfplumber |
| PDF为扫描件（无文本层） | 提示需要使用OCR预处理 |
| 某字段未在文中提及 | 标注"文中未明确提及"，不推测 |

---

## 依赖

- Python >= 3.8
- PyMuPDF（优先）或 pdfplumber（至少安装其一）
