# AscendC Development Knowledge Base

CANN (Compute Architecture for Neural Networks) AscendC 算子开发知识库，用于 Claude Code 辅助 AscendC 算子开发。

## 目录结构

```
references/                          # 知识文档（纯 Markdown，无图片）
  api_reference_docs/                # API 参考文档（基础API/高阶API/Utils）
  basic_knowledge_docs/              # 编程指南 + 算子实践参考
  basic_data_api_docs/               # Host 侧 API（Tiling/InferShape）
  troubleshooting_docs/              # 错误码 / 故障排查
  log_reference_docs/                # 日志参考
  910D_knowledge_extra/              # 910D (351x) 特有知识

scripts/                             # 工具脚本
  convert_images_to_text.py          # 图片转文本工具
```

## 图片转文本工具

`scripts/convert_images_to_text.py` 通过 LLM Vision API 将文档中的图片引用转为纯文本：

- 公式图 → LaTeX 数学表达式
- 示意图/架构图 → ASCII art + 文字标注
- 图标 → 删除引用

### 环境变量

| 变量 | 说明 | 默认值 |
|---|---|---|
| `ANTHROPIC_API_KEY` | API 密钥 | （必须设置） |
| `ANTHROPIC_BASE_URL` | API 地址（支持 litellm proxy 等兼容端点） | `https://api.anthropic.com` |

### 用法

```bash
# 扫描统计图片引用
python3 scripts/convert_images_to_text.py --scan

# 清理图标引用（不需要转换的小图标）
python3 scripts/convert_images_to_text.py --clean-icons

# 全量转换（默认 gpt-5.4, 16 线程并发）
python3 scripts/convert_images_to_text.py --convert

# 指定文件或目录
python3 scripts/convert_images_to_text.py --convert path/to/file.md
python3 scripts/convert_images_to_text.py --convert path/to/dir/

# 小批量测试
python3 scripts/convert_images_to_text.py --convert --limit 5

# 转换完成后清理图片文件
python3 scripts/convert_images_to_text.py --cleanup
```

### 转换标记

已转换的内容在 Markdown 中用 `<!-- img2text -->` 标记，重复运行会自动跳过已转换的图片。
