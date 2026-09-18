# 调研方向分类表

根据报告主题判断归属目录。匹配最主要的方向，跨方向时在标签中标注。

| 目录 | 方向 | 涵盖内容 | 关键词示例 |
|------|------|----------|-----------|
| `video-generation/` | 视频生成 | 视频模型、训练框架、推理加速、视频编辑、T2V/I2V | 视频、video、T2V、I2V、Sora、H3、Wan、Hunyuan、可灵、即梦 |
| `image-generation/` | 图像生成 | 图像模型、编辑、ControlNet、风格迁移、T2I | 图像、image、T2I、SD、FLUX、DALL-E、Midjourney、ControlNet |
| `llm-training/` | LLM 训练 | 预训练、SFT、RL、对齐、训练框架、大语言模型 | LLM、大模型、预训练、SFT、RLHF、GRPO、对齐、GPT、Qwen、Llama |
| `multimodal/` | 多模态 | VLM、音视频统一、多模态理解与生成、跨模态 | 多模态、multimodal、VLM、图文、音视频统一、Omni、GPT-4o |
| `npu-ecosystem/` | NPU/硬件 | 昇腾、CUDA、ROCm、硬件对比、算力生态、芯片 | NPU、昇腾、Ascend、CUDA、ROCm、GPU、芯片、算力、华为、寒武纪 |
| `industry-analysis/` | 行业分析 | 竞品、市场、公司调研、行业趋势、商业模式 | 行业、市场、竞品、公司、趋势、商业模式、融资、估值 |
| `inference-optimization/` | 推理优化 | 量化、蒸馏、加速、部署、推理引擎、vLLM、SGLang | 推理、量化、蒸馏、加速、部署、vLLM、SGLang、TensorRT、KV cache |
| `agent-infra/` | Agent 基础设施 | Agent 框架、多 Agent 编排、工具调用、MCP、Agent 评测 | Agent、智能体、多Agent、MCP、工具调用、编排、评测、AutoGen、LangGraph |

## 判断规则

1. 报告主题明确匹配某一行的关键词 → 直接归入该目录
2. 报告涉及多个方向 → 选内容占比最大的方向，标签中标注其他方向
3. 报告主题不匹配任何现有方向 → 创建新目录（英文小写连字符），并在此表和 README 中添加
4. 训练框架类报告：如果框架主要用于视频模型训练 → `video-generation/`；如果是通用 LLM 训练框架 → `llm-training/`
5. 硬件/芯片相关调研 → `npu-ecosystem/`，即使涉及模型训练也优先归硬件方向
