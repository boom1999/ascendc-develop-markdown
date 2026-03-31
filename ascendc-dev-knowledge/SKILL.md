---
name: ascendc-dev-knowledge
description: "AscendC operator development knowledge base. Use when looking up AscendC API signatures, programming concepts, operator examples, error codes, or troubleshooting. Covers Kernel-side APIs, Host-side data structures, programming guides, performance optimization patterns, and fault diagnosis. Triggers on AscendC API, operator development, Tiling, InferShape, DataCopy, error codes, troubleshooting, programming guide."
---

# AscendC Knowledge Base

## 前置检查

使用前先确认知识库数据是否就绪：

```bash
ls ./references/api_reference_docs/INDEX.md ./references/basic_knowledge_docs/INDEX.md \
   ./references/basic_data_api_docs/INDEX.md ./references/troubleshooting_docs/INDEX.md \
   ./references/log_reference_docs/INDEX.md 2>/dev/null | wc -l
```

- 结果为 **5**：知识库就绪，可直接检索。
- 结果 < 5：需要先构建知识库，执行以下命令：

```bash
python3 ./scripts/ascendc_spider.py -v 850
python3 ./scripts/restructure_kb.py -v 850
```

## 知识库路径

知识库数据位于本 skill 的 `./references/` 目录内：

| 目录 | 说明 |
|------|------|
| `./references/cache/` | 原始的文档（清洗后默认删除） |
| `./references/*_docs/` | 清洗后的知识库（检索目标） |
| `./references/910D_knowledge_extra/` | 910D (Ascend 351x) 专用知识库（独立外挂，无需构建） |

支持 850 和 900beta1 等多个 CANN 版本，通过 `-v` 参数切换，共用同一组目录名。

## 知识库结构

结构因版本而异，以下为典型布局（具体子目录以实际 INDEX.md 为准）：

```
./references/
├── api_reference_docs/         — Ascend C 算子开发接口
│   ├── INDEX.md
│   ├── 基础API/                # 矢量计算/矩阵计算/数据搬运/同步控制/Kernel Tiling
│   ├── 高阶API/                # Matmul/Conv3D/数学计算/归约/排序/量化
│   ├── Utils_API/              # Tiling/RTC/平台信息/C++标准库
│   ├── 基础数据结构/           # LocalTensor/GlobalTensor/Layout/Coordinate
│   └── 其他数据类型/           # TensorDesc/TPosition
│
├── basic_knowledge_docs/       — 编程指南 + 算子实践参考 + 入门教程
│   ├── INDEX.md
│   ├── 编程指南/                # 概念原理/编程模型/范式/硬件实现/编译运行/调试
│   ├── 算子实践参考/            # SIMD实现(矢量/矩阵/融合) + 性能优化 + 功能调试
│   └── 入门教程/                # 快速入门/HelloWorld/Add算子
│
├── basic_data_api_docs/        — Host 侧基础数据结构和接口
│   ├── INDEX.md
│   ├── gert命名空间/           # TilingContext/Shape/InferShapeContext/TensorV2
│   └── ge命名空间/             # AscendString/OpRegistrationData/KernelLaunchInfo
│
├── troubleshooting_docs/       — 故障处理
│   ├── INDEX.md
│   ├── 错误码参考/              # GE/RTS/HCCL/AI_CPU/FE/Driver 等模块
│   ├── 典型故障专题/            # AI Core Error/OOM/进程中断/进程卡住
│   └── 故障定位工具/            # asys 工具
│
├── log_reference_docs/         — 日志参考
│   ├── INDEX.md
│   └── FAQ/
│
└── 910D_knowledge_extra/       — 910D (Ascend 351x) 专用文档
    ├── *.md                     # ~2000+ 扁平 MD 文件（API/架构/迁移指南）
    └── figures/                 # 配图
```

## Local API Documentation

| Docs | Search Guide | Use for |
|------|-------------|---------|
| `api_reference_docs/` | `references/api-reference.md` | Kernel-side API signatures, parameters, constraints, data type support, mask, sync control |
| `basic_knowledge_docs/` | `references/basic-knowledge.md` | AI Core architecture, programming paradigms, operator examples, Tiling strategies, performance optimization |
| `basic_data_api_docs/` | `references/basic-data-api.md` | TilingContext, InferShape, Shape, TensorDesc, OpDef registration |
| `troubleshooting_docs/` | `references/troubleshooting.md` | Error codes (EZ/EE/EG/...), AI Core Error, OOM, process hang/crash, asys tool |
| `log_reference_docs/` | `references/log-reference.md` | Log level configuration, plog framework, log FAQ |
| `910D_knowledge_extra/` | — (flat, grep directly) | 910D/351x architecture, 220x→351x migration, RegBase programming, SIMD VF functions, MicroAPI |

Search guides contain grep/find/cat examples and common query patterns. Start with `cat INDEX.md` for any section.

## 快速检索速查

```bash
# 找 API 文档
find "./references/api_reference_docs" -name "DataCopy*.md"
grep -rl "void Sqrt" "./references/api_reference_docs/"

# 找编程概念
grep -rl "DoubleBuffer\|双缓冲" "./references/basic_knowledge_docs/编程指南/"

# 找性能优化
find "./references/basic_knowledge_docs/算子实践参考/SIMD算子性能优化/" -name "*.md"

# 找 Host 侧接口
grep -rl "GetInputShape" "./references/basic_data_api_docs/gert命名空间/"

# 找错误码
grep -rl "EZ9999" "./references/troubleshooting_docs/错误码参考/"

# 看某个 section 的完整索引
cat "./references/api_reference_docs/INDEX.md"

# 找 910D 专用文档
find "./references/910D_knowledge_extra" -maxdepth 1 -name "*.md" | grep -i "RegBase\|351x\|迁移"
grep -rl "VF函数\|MicroAPI\|RegBase" "./references/910D_knowledge_extra/"
```

## 知识库构建工具

脚本位于 `./scripts/`，详细参数见各脚本 `--help`。

### Pipeline: 爬虫（spider）→ cache → 清洗（restructure）→ *_docs

```
ascendc_spider.py          restructure_kb.py
   -v {version}               -v {version}
      │                          │
      ▼                          ▼
  references/cache/        references/*_docs/
  (raw pages + images)     (hierarchical, cleaned)
```

清洗默认删除 cache（`--keep-cache` 可保留）。

### 完整 Rebuild 流程

```bash
# 1. 爬取
python3 ./scripts/ascendc_spider.py -v 850
# 2. 清洗（自动删除 cache）
python3 ./scripts/restructure_kb.py -v 850
```

> **注意**: `910D_knowledge_extra/` 为独立外挂知识库，不参与上述构建流程。
