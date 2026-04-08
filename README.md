# AscendC Development Documentation + Claude Code Skill

Huawei AscendC operator development documentation converted to searchable markdown, with a Claude Code skill for NPU operator development.

## Technical Details

| 项目 | 数值 |
|---|---|
| Markdown 文档数 | 3,742 |
| 知识库大小 | ~35 MB（纯 Markdown，图片已转为文本） |
| 数据来源 | [hiascend.com](https://www.hiascend.com) 官方文档 |
| CANN 版本 | 8.5.0 Community Edition |
| 图片处理 | 1,927 张图片转为 ASCII art / LaTeX / 文字描述，140 个图标引用已清理 |

**Size Breakdown**:

| 分类 | 文件数 | 大小 |
|---|---|---|
| API Reference (`api_reference_docs/`) | 863 | ~6.9 MB |
| Programming Guide (`basic_knowledge_docs/`) | 164 | ~3.1 MB |
| Host-side Data API (`basic_data_api_docs/`) | 596 | ~2.7 MB |
| Troubleshooting (`troubleshooting_docs/`) | 72 | ~532 KB |
| Log Reference (`log_reference_docs/`) | 8 | ~52 KB |
| 910D Extra Knowledge (`910D_knowledge_extra/`) | ~2,000 | ~22 MB |

## What's Here

1. **API Reference** — Complete Kernel-side API documentation: function signatures, parameters, constraints, data type support. Covers vector/matrix compute, data transfer, sync control, Matmul, Conv3D, quantization, etc.

2. **Programming Guide + Operator Examples** — AI Core architecture, programming paradigms (CopyIn → Compute → CopyOut), SIMD operator implementation, performance optimization (Tiling, double buffering, bank conflict avoidance), getting started tutorials.

3. **Host-side Data API** — `gert` namespace (TilingContext, InferShape, Shape, TensorV2), `ge` namespace (AscendString, OpRegistrationData), C interface wrappers.

4. **Troubleshooting** — Error code reference (GE/RTS/HCCL/AI_CPU/FE/Driver), AI Core Error diagnosis, OOM analysis, process hang/crash, asys diagnostic tool guide.

5. **Log Reference** — Log level configuration, plog framework, FAQ.

6. **910D (Ascend 351x) Extra Knowledge** — 220x → 351x architecture migration, RegBase programming, SIMD VF functions, MicroAPI reference, 351x-specific API documentation.

7. **AscendC Development Skill** for Claude Code — API lookup, programming concept search, error code diagnosis, prerequisite check.

## Why

Huawei's official AscendC documentation is spread across hundreds of HTML pages, requires clicking through multi-level navigation, and is not searchable with standard tools. This conversion enables:

- `grep -r "void DataCopy" references/api_reference_docs/` instead of clicking through pages
- `grep -rl "DoubleBuffer" references/basic_knowledge_docs/` for concept lookup
- `grep -rl "EZ9999" references/troubleshooting_docs/` for error code diagnosis
- Direct file access for AI tools (Claude Code, Copilot)
- Offline reference with hierarchical organization

## Structure

```
ascendc-dev-knowledge/                       # Portable Claude Code skill (~35MB)
├── SKILL.md                                 # Main skill definition
├── scripts/
│   ├── ascendc_spider.py                    # Step 1: 爬虫 — 从 hiascend.com 抓取文档
│   ├── restructure_kb.py                    # Step 2: 清洗 — 整理目录结构，清理内容
│   └── convert_images_to_text.py            # Step 3: 图片转文本 — LLM Vision API 转换
└── references/
    ├── api_reference_docs/                  # 863 files — Kernel-side APIs
    │   ├── INDEX.md
    │   ├── 基础API/                         # Vector/matrix compute, data transfer, sync
    │   ├── 高阶API/                         # Matmul, Conv3D, quantization, sorting
    │   ├── Utils_API/                       # Tiling macros, RTC, platform info
    │   ├── 基础数据结构/                    # LocalTensor, GlobalTensor, Layout
    │   └── 其他数据类型/                    # TensorDesc
    ├── basic_knowledge_docs/                # 164 files — Programming guide + examples
    │   ├── INDEX.md
    │   ├── 编程指南/                        # Concepts, paradigms, compilation, debugging
    │   ├── 算子实践参考/                    # SIMD implementation, performance optimization
    │   └── 入门教程/                        # Getting started, HelloWorld, Add operator
    ├── basic_data_api_docs/                 # 596 files — Host-side interfaces
    │   ├── INDEX.md
    │   ├── gert命名空间/                    # TilingContext, Shape, InferShape, TensorV2
    │   └── ge命名空间/                      # AscendString, OpRegistrationData
    ├── troubleshooting_docs/                # 72 files — Fault handling
    │   ├── INDEX.md
    │   ├── 错误码参考/                      # GE/RTS/HCCL/AI_CPU/FE/Driver errors
    │   ├── 典型故障专题/                    # AI Core Error, OOM, hang, crash
    │   └── 故障定位工具/                    # asys tool
    ├── log_reference_docs/                  # 8 files — Log configuration
    │   └── INDEX.md
    └── 910D_knowledge_extra/                # ~2000 files — 910D/351x specific

README.md                                    # This file
```

## Knowledge Base Usage

**IMPORTANT**: Before developing any AscendC code, query the knowledge base via the `ascendc-dev-knowledge` skill.

### First Use — Load Core Indexes

After environment check passes, **must read** the following two core indexes:

1. `skills/ascendc-dev-knowledge/references/basic_knowledge_docs/INDEX.md` (programming guide + operator practice)
2. `skills/ascendc-dev-knowledge/references/api_reference_docs/INDEX.md` (API signatures/parameters/constraints)

### On-Demand Lookup

Read the corresponding INDEX.md based on task type, then follow paths to specific documents:

| Scenario | INDEX.md Path (relative to `~/.claude/` or `.claude/`) |
|---|---|
| API signatures/parameters/constraints | `skills/ascendc-dev-knowledge/references/api_reference_docs/INDEX.md` |
| Programming guide + operator practice | `skills/ascendc-dev-knowledge/references/basic_knowledge_docs/INDEX.md` |
| Host-side APIs (Tiling/InferShape) | `skills/ascendc-dev-knowledge/references/basic_data_api_docs/INDEX.md` |
| Error codes / troubleshooting | `skills/ascendc-dev-knowledge/references/troubleshooting_docs/INDEX.md` |
| Log reference | `skills/ascendc-dev-knowledge/references/log_reference_docs/INDEX.md` |
| 910D specific (351x) | `skills/ascendc-dev-knowledge/references/910D_knowledge_extra/` (grep search) |

## Using the Skill

Install:
```bash
cp -r ascendc-dev-knowledge ~/.claude/skills/ascendc-dev-knowledge
```

Or symlink for development:
```bash
ln -s $(pwd)/ascendc-dev-knowledge ~/.claude/skills/ascendc-dev-knowledge
```

The skill activates automatically for AscendC work. Ask Claude:
- "DataCopy 的参数是什么？"
- "双缓冲怎么实现？"
- "EZ9999 错误码什么意思？"
- "TilingContext 有哪些方法？"
- "351x 架构和 220x 有什么区别？"
- "Matmul 高阶 API 怎么调用？"

## Build Pipeline

知识库通过三个脚本分步构建。可以在 Step 2 之后直接使用（保留原始图片），也可以继续执行 Step 3 将图片转为纯文本。

```
hiascend.com ──→ [Step 1: 爬虫] ──→ [Step 2: 清洗] ──→ 可直接使用（含图片，~88MB）
                                                    ──→ [Step 3: 图片转文本] ──→ 纯文本版（~35MB）
```

### Step 1: 爬虫 — `ascendc_spider.py`

从 hiascend.com 爬取文档页面，将 HTML 转为 Markdown，下载关联图片。

```bash
# 爬取 CANN 8.5.0 全部六个分类
python3 ascendc-dev-knowledge/scripts/ascendc_spider.py -v 850

# 仅爬取指定分类
python3 ascendc-dev-knowledge/scripts/ascendc_spider.py -v 850 -s api_reference

# 指定输出目录
python3 ascendc-dev-knowledge/scripts/ascendc_spider.py -v 850 -o /path/to/output
```

支持的文档分类：`api_reference`、`basic_knowledge`、`basic_data_api`、`troubleshooting`、`log_reference`

支持的版本：`850` (CANN 8.5.0)、`900beta1` (CANN 9.0.0 beta1)

### Step 2: 清洗 — `restructure_kb.py`

将爬虫的扁平输出整理为层级目录结构，清理冗余内容（去重标题、剥离模板文本、修复链接等），生成每个分类的 `INDEX.md` 索引文件。

```bash
# 整理 CANN 8.5.0 全部分类
python3 ascendc-dev-knowledge/scripts/restructure_kb.py -v 850

# 仅整理指定分类
python3 ascendc-dev-knowledge/scripts/restructure_kb.py -v 850 -s api_reference
```

**完成此步骤后即可使用知识库**（包含原始图片，约 88MB）。

Cleanup pipeline details:
- **Dedup**: Removes TOC-only pages (~35% of raw content)
- **De-noise**: Strips invisible characters (`\xa0`, `\u200b`, `\ufeff`), removes anchor artifacts
- **Link cleanup**: Strips internal cross-reference URLs, preserving type/function names as plain text
- **Boilerplate removal**: Removes dead link remnants, empty sections, repeated constraint references
- **Structure**: Builds hierarchical directory tree from breadcrumb navigation, generates tree-form INDEX.md

**Note**: `910D_knowledge_extra/` is an independently maintained knowledge base and does not participate in the spider/restructure pipeline.

### Step 3（可选）: 图片转文本 — `convert_images_to_text.py`

通过 LLM Vision API 将文档中的图片引用转为纯文本：

- 公式图 → LaTeX 数学表达式
- 示意图/架构图 → ASCII art + 文字标注
- 图标 → 删除引用

#### 环境变量

| 变量 | 说明 | 默认值 |
|---|---|---|
| `ANTHROPIC_API_KEY` | API 密钥 | （必须设置） |
| `ANTHROPIC_BASE_URL` | API 地址（支持 litellm proxy 等兼容端点） | `https://api.anthropic.com` |

#### 用法

```bash
# 1. 扫描统计图片引用
python3 ascendc-dev-knowledge/scripts/convert_images_to_text.py --scan

# 2. 清理图标引用
python3 ascendc-dev-knowledge/scripts/convert_images_to_text.py --clean-icons

# 3. 全量转换（默认 gpt-5.4, 16 线程并发）
python3 ascendc-dev-knowledge/scripts/convert_images_to_text.py --convert

# 4. 转换完成后清理图片文件
python3 ascendc-dev-knowledge/scripts/convert_images_to_text.py --cleanup

# 5. 验证无残留
python3 ascendc-dev-knowledge/scripts/convert_images_to_text.py --scan
```

也可以指定文件或目录进行局部操作：

```bash
python3 ascendc-dev-knowledge/scripts/convert_images_to_text.py --convert path/to/file.md
python3 ascendc-dev-knowledge/scripts/convert_images_to_text.py --convert path/to/dir/
python3 ascendc-dev-knowledge/scripts/convert_images_to_text.py --convert --limit 5
python3 ascendc-dev-knowledge/scripts/convert_images_to_text.py --convert --model gpt-5.4 -w 16
```

已转换的内容用 `<!-- img2text -->` 标记，重复运行会自动跳过。

## Use Cases

- **AscendC operator development** — API lookup, coding patterns, tiling strategies
- **Host-side development** — TilingContext, InferShape, OpDef registration
- **Performance optimization** — Double buffering, bank conflict, pipeline scheduling
- **Debugging** — Error code lookup, AI Core Error diagnosis, OOM analysis
- **910D/351x development** — Architecture migration, RegBase programming
- **Learning AscendC** — Tutorials, programming paradigms, example operators

---

Unofficial conversion for convenience. Refer to Huawei's official documentation for authoritative reference:
- [Ascend Community](https://www.hiascend.com/)
- [CANN Documentation](https://www.hiascend.com/document)
