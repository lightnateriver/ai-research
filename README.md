# AI Research Notes

个人技术调研报告库。按调研方向分类归档，所有报告采用 HTML 格式，华为配色浅色系规范。

## 📂 目录结构

```
ai-research/
├── README.md                        # 本文件 - 报告索引（所有报告必须在此登记）
├── video-generation/               # 视频生成方向
├── image-generation/               # 图像生成方向
├── llm-training/                   # LLM 训练方向
├── multimodal/                     # 多模态方向
├── npu-ecosystem/                  # NPU/硬件生态方向
├── industry-analysis/              # 行业/竞品分析方向
├── inference-optimization/         # 推理优化方向
├── agent-infra/                    # Agent 基础设施方向
├── skills/                         # 自动化 Skill
│   └── github-research/            # 调研报告归档工作流 Skill
├── templates/                      # 报告模板
│   └── research-template.html
└── assets/                         # 图片/图表资源
```

## 🛠️ Skills

### github-research

调研报告归档与上传的标准化工作流 Skill。核心能力：

- **四步闭环**：判断方向 → 生成华为配色浅色系 HTML → push 到 GitHub → 更新 README 索引
- **报告更新**：支持删旧传新，刷新已有报告时自动删除旧文件、用新日期命名上传、更新索引链接
- **Push 前 README 审视**：每次 push 前强制检查是否需要更新根目录 README（结构变化、索引更新、新增说明等）
- **方向自动归档**：9 个预设方向，不匹配时自动创建新方向目录
- **质量检查**：上传前自动检查配色规范、日期、结论章节、数据来源等

详见 [`skills/github-research/SKILL.md`](skills/github-research/SKILL.md)。

## 📋 归档规则

新调研报告进入时，按以下规则归档：

1. **判断方向**：根据调研主题判断属于哪个方向目录
2. **已有方向**：直接归档到对应目录
3. **新方向**：如果现有目录都不匹配，创建新的方向目录
4. **文件命名**：`YYYY-MM-关键词.html`（日期为报告完成日期）
5. **⚠️ 强制更新索引**：每篇报告归档后，**必须**在下方「报告索引」表中添加一行，包含日期、标题、方向、标签、链接。未在索引中登记的报告视为未归档。

### 方向说明

| 目录 | 方向 | 涵盖内容 |
|------|------|----------|
| `video-generation/` | 视频生成 | 视频模型、训练框架、推理加速、视频编辑 |
| `image-generation/` | 图像生成 | 图像模型、编辑、ControlNet、风格迁移 |
| `llm-training/` | LLM 训练 | 预训练、SFT、RL、对齐、训练框架 |
| `multimodal/` | 多模态 | VLM、音视频统一、多模态理解与生成 |
| `npu-ecosystem/` | NPU/硬件 | 昇腾、CUDA、ROCm、硬件对比、算力生态 |
| `industry-analysis/` | 行业分析 | 竞品、市场、公司调研、行业趋势 |
| `inference-optimization/` | 推理优化 | 量化、蒸馏、加速、部署、推理引擎 |
| `agent-infra/` | Agent 基础设施 | Agent 框架、多 Agent 编排、工具调用、MCP、Agent 评测 |

## 🎨 HTML 报告规范

### 配色规范（华为配色，浅色系）

| 用途 | 色值 | 说明 |
|------|------|------|
| 主色 | `#C7000B` | 华为红，用于标题、强调、表头 |
| 主色浅 | `#E8384F` | 辅助强调 |
| 主色背景 | `#FFF0F1` | 浅红背景，用于高亮区块 |
| 正文文字 | `#1A1A1A` | 主文字 |
| 次要文字 | `#555555` | 说明文字 |
| 弱化文字 | `#888888` | 注释、日期 |
| 页面背景 | `#F7F8FA` | 浅灰背景 |
| 卡片背景 | `#FFFFFF` | 白色卡片 |
| 边框 | `#E8E8E8` | 浅灰边框 |

### 结构规范

- 文件名：`YYYY-MM-关键词.html`
- 页面顶部：标题 + 日期 + 标签
- 正文：分章节，每章有明确标题
- 表格：表头使用华为红背景白字
- 结论：每篇报告末尾有关键结论总结
- 数据来源：注明数据获取时间和来源

### 模板

报告模板见 [`templates/research-template.html`](templates/research-template.html)，新报告直接复制使用。

## 📑 报告索引

> 所有已归档报告必须在此登记，按日期倒序排列。

| 日期 | 标题 | 方向 | 标签 | 链接 |
|------|------|------|------|------|
| 2026-09-18 | Seedance 1.0 深度技术调研报告 | video-generation | #视频生成 #Seedance #DiT #RLHF #推理加速 #多镜头 | [HTML](video-generation/2026-09-seedance-1.0.html) |
| 2026-09-18 | Seedance 1.5 pro 深度技术调研报告 | video-generation | #视频生成 #Seedance #音视频联合 #双分支DiT #音频RLHF #唇形同步 | [HTML](video-generation/2026-09-seedance-1.5-pro.html) |
| 2026-09-18 | Seedance 2.0 深度技术调研报告 | video-generation | #视频生成 #Seedance #多模态 #R2V #编辑 | [HTML](video-generation/2026-09-seedance-2.0.html) |
| 2026-09-18 | Seedance 系列版本演进对比报告 | video-generation | #视频生成 #Seedance #版本对比 #技术演进 | [HTML](video-generation/2026-09-seedance-evolution.html) |
| 2026-09-18 | 主流云厂商 KV Cache 存储商业模式调研报告 | inference-optimization | #KV-Cache #商业模式 #推理优化 #云厂商 #专属存储 | [HTML](inference-optimization/2026-09-kv-cache-storage.html) |
| 2026-09-18 | MiniMax H3 开源训练框架调研报告 | video-generation | #视频生成 #训练框架 #SFT #RL #NPU #MoE #蒸馏 | [HTML](video-generation/2026-09-minimax-h3-training-frameworks.html) |

---

*最后更新：2026-09-18 | 已归档报告：6 篇*
