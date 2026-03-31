# AscendC Development Documentation + Claude Code Skill

Huawei AscendC operator development documentation converted to searchable markdown, with a Claude Code skill for NPU operator development.

## What's Here

1. **API Reference** (863 markdown files)
   - Complete Kernel-side API documentation
   - Function signatures, parameters, constraints, data type support
   - Covers vector/matrix compute, data transfer, sync control, Matmul, Conv3D, quantization, etc.

2. **Programming Guide + Operator Examples** (164 markdown files)
   - AI Core architecture, programming paradigms (CopyIn → Compute → CopyOut)
   - SIMD operator implementation (vector, matrix, fused)
   - Performance optimization (Tiling, double buffering, bank conflict avoidance)
   - Getting started tutorials (HelloWorld, Add operator)

3. **Host-side Data API** (596 markdown files)
   - `gert` namespace: TilingContext, InferShape, Shape, TensorV2, CompileTimeTensorDesc
   - `ge` namespace: AscendString, OpRegistrationData, KernelLaunchInfo
   - C interface wrappers

4. **Troubleshooting** (72 markdown files)
   - Error code reference (GE/RTS/HCCL/AI_CPU/FE/Driver)
   - AI Core Error diagnosis, OOM analysis, process hang/crash
   - asys diagnostic tool guide

5. **Log Reference** (8 markdown files)
   - Log level configuration, plog framework, FAQ

6. **910D (Ascend 351x) Extra Knowledge** (~2000 markdown files)
   - 220x → 351x architecture migration
   - RegBase programming, SIMD VF functions, MicroAPI reference
   - 351x-specific API documentation

7. **AscendC Development Skill** for Claude Code
   - API lookup with search guides per section
   - Programming concept and pattern search
   - Error code diagnosis
   - Prerequisite check (auto-detect if knowledge base needs building)

## Why

Huawei's official AscendC documentation is:
- Spread across hundreds of HTML pages on the Ascend Community website
- Requires clicking through multi-level navigation to find API details
- Not searchable across sections with standard tools

This conversion enables:
- `grep -r "void DataCopy" references/api_reference_docs/` instead of clicking through pages
- `grep -rl "DoubleBuffer" references/basic_knowledge_docs/` for concept lookup
- `grep -rl "EZ9999" references/troubleshooting_docs/` for error code diagnosis
- Direct file access for AI tools (Claude Code, Copilot)
- Offline reference with hierarchical organization

**Example 1**: Find DataCopy API signature:

```bash
$ grep -r "void DataCopy" ascendc-dev-knowledge/references/api_reference_docs/基础API/数据搬运/
DataCopy.md: void DataCopy(const LocalTensor<T>& dstLocal, const GlobalTensor<T>& srcGlobal, ...
```

**Example 2**: Look up error code EZ9999:

```bash
$ grep -rl "EZ9999" ascendc-dev-knowledge/references/troubleshooting_docs/
```

**Example 3**: Find 351x migration guide:

```bash
$ find ascendc-dev-knowledge/references/910D_knowledge_extra -name "*351x*"
351x架构迁移指导.md
```

## Structure

```
ascendc-dev-knowledge/                       # Portable Claude Code skill (~88MB)
├── SKILL.md                                 # Main skill definition
├── scripts/
│   ├── ascendc_spider.py                    # Web scraper for Ascend Community docs
│   └── restructure_kb.py                    # Raw → cleaned knowledge base converter
└── references/
    ├── api-reference.md                     # Search guide: API reference
    ├── basic-knowledge.md                   # Search guide: programming concepts
    ├── basic-data-api.md                    # Search guide: host-side data API
    ├── troubleshooting.md                   # Search guide: error codes & faults
    ├── log-reference.md                     # Search guide: logging
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
    │   ├── INDEX.md
    │   └── FAQ/
    └── 910D_knowledge_extra/                # ~2000 files — 910D/351x specific
        ├── *.md                             # Flat structure (API, architecture, migration)
        └── figures/                         # Diagrams

README.md                                    # This file
```

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

Claude searches the local documentation and provides answers with file references.

## Search Examples

### API Reference

Find a specific API:
```bash
find ascendc-dev-knowledge/references/api_reference_docs -name "Sqrt.md"
```

Search function signatures:
```bash
grep -r "void DataCopy" ascendc-dev-knowledge/references/api_reference_docs/基础API/数据搬运/
```

Find data type support:
```bash
grep -rl "bfloat16" ascendc-dev-knowledge/references/api_reference_docs/
```

### Programming Concepts

Find Tiling strategies:
```bash
find ascendc-dev-knowledge/references/basic_knowledge_docs -name "*Tiling*" -o -name "*tiling*"
```

Search for double buffering:
```bash
grep -rl "DoubleBuffer\|双缓冲" ascendc-dev-knowledge/references/basic_knowledge_docs/
```

### Host-side API

Look up TilingContext methods:
```bash
find ascendc-dev-knowledge/references/basic_data_api_docs/gert命名空间/TilingContext/ -name "*.md"
```

### Troubleshooting

Search error codes:
```bash
grep -rl "EZ9999" ascendc-dev-knowledge/references/troubleshooting_docs/错误码参考/
```

### 910D Knowledge

Find migration docs:
```bash
find ascendc-dev-knowledge/references/910D_knowledge_extra -maxdepth 1 -name "*迁移*"
```

Search RegBase programming:
```bash
grep -rl "RegBase" ascendc-dev-knowledge/references/910D_knowledge_extra/
```

## Regenerating

The knowledge base is built from Huawei's Ascend Community documentation in a two-step pipeline:

```bash
# 1. Scrape raw pages from Ascend Community website
python3 ascendc-dev-knowledge/scripts/ascendc_spider.py -v 850

# 2. Clean and restructure into hierarchical knowledge base (auto-deletes cache)
python3 ascendc-dev-knowledge/scripts/restructure_kb.py -v 850

# Keep raw cache for inspection:
python3 ascendc-dev-knowledge/scripts/restructure_kb.py -v 850 --keep-cache
```

**Spider**: Crawls 5 documentation sections from the Ascend Community website, downloads pages and images, preserves tables and code blocks, generates INDEX.md per section.

**Restructure** (cleanup pipeline):

- **Dedup**: Removes TOC-only pages (~35% of raw content) that are just navigation link lists — keeps only pages with actual API details
- **De-noise**: Strips invisible characters (`\xa0` non-breaking space, `\u200b` zero-width space, `\ufeff` BOM), removes `#ZH-CN_TOPIC` anchor artifacts
- **Link cleanup**: Strips all internal cross-reference URLs and absolute hiascend.com links, preserving type names and function names as plain text — makes grep/find searches clean and precise
- **Boilerplate removal**: Removes dead link remnants (`更多样例可参考LINK`), empty return value sections (`#### 返回值说明 → 无`), and repeated constraint references — surfaces the real API logic
- **Structure**: Builds hierarchical directory tree from breadcrumb navigation, deduplicates H1 headings, moves images alongside their pages, generates tree-form INDEX.md per section

Supported versions: `850` (CANN 8.5.0), `900beta1` (CANN 9.0.0 beta1). Switch with `-v`.

**Note**: `910D_knowledge_extra/` is an independently maintained knowledge base and does not participate in the spider/restructure pipeline.

## Technical Details

**API Reference (CANN 8.5.0)**:
- Files: 863 markdown + INDEX.md
- Size: 16 MB (including images)
- Source: https://www.hiascend.com/ (Ascend C API Reference)

**Programming Guide (CANN 8.5.0)**:
- Files: 164 markdown + INDEX.md
- Size: 18 MB (including images)
- Source: https://www.hiascend.com/ (AscendC Programming Guide)

**Host-side Data API (CANN 8.5.0)**:
- Files: 596 markdown + INDEX.md
- Size: 2.9 MB
- Source: https://www.hiascend.com/ (Basic Data API Reference)

**Troubleshooting (CANN 8.5.0)**:
- Files: 72 markdown + INDEX.md
- Size: 1.6 MB
- Source: https://www.hiascend.com/ (Troubleshooting Guide)

**Log Reference (CANN 8.5.0)**:
- Files: 8 markdown + INDEX.md
- Size: 176 KB
- Source: https://www.hiascend.com/ (Log Reference)

**910D Extra Knowledge**:
- Files: ~2000 markdown
- Size: 50 MB (including figures)
- Covers: Ascend 351x architecture, 220x→351x migration, RegBase, MicroAPI

**Total skill size**: ~88 MB

**License**: Documentation content originates from Huawei Ascend Community.

The skill uses Claude Code's progressive disclosure: `SKILL.md` is always loaded, search guides and documentation are searched on-demand via grep/find.

## Use Cases

- **AscendC operator development** — API lookup, coding patterns, tiling strategies
- **Host-side development** — TilingContext, InferShape, OpDef registration
- **Performance optimization** — Double buffering, bank conflict, pipeline scheduling
- **Debugging** — Error code lookup, AI Core Error diagnosis, OOM analysis
- **910D/351x development** — Architecture migration, RegBase programming
- **Learning AscendC** — Tutorials, programming paradigms, example operators
- **Training AI models** on AscendC/NPU development

---

Unofficial conversion for convenience. Refer to Huawei's official documentation for authoritative reference:
- [Ascend Community](https://www.hiascend.com/)
- [CANN Documentation](https://www.hiascend.com/document)
