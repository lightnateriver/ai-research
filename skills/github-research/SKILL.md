---
name: github-research
description: 调研报告归档与上传到 GitHub 仓库 ai-research 的标准化工作流。将调研内容生成华为配色浅色系 HTML，按方向分类归档到对应目录，push 到 GitHub，并强制更新 README 索引表。支持新报告归档和已有报告刷新更新（删旧传新）。当用户要求"归档调研报告""上传报告到 github""把这份调研存起来""更新报告索引""push 调研报告""刷新报告""更新这份报告""重新上传报告"时使用。
---

# GitHub Research — 调研报告归档工作流

将调研报告标准化归档到 GitHub 仓库。

## 配置

使用前确认以下参数（默认值如下，按实际仓库修改）：

- GitHub 用户名（owner）：`lightnateriver`
- 仓库名（repo）：`ai-research`
- 分支（branch）：`main`
- 仓库克隆 URL（带 token）：`https://{owner}:{TOKEN}@github.com/{owner}/{repo}.git`

## 核心四步闭环

```
判断方向 → 生成规范 HTML → push 到 GitHub → 更新 README 索引
```

每一步都是强制的，缺一不可。

## Push 方案选型（重要）

| 方案 | 适用场景 | 优势 | 限制 |
|------|---------|------|------|
| **纯 Token + Git（首选）** | 所有场景，尤其大文件（>50K字符） | 不读文件内容、不污染上下文、支持任意大小、批量高效 | 需要用户提供 GitHub Token |
| MCP 工具（备选） | 小文件（<50K字符）或用户不愿提供 Token 时 | OAuth 已认证，无需 Token | content 参数受 LLM 输出长度限制（~25K tokens），大文件完全无法上传 |

**默认使用纯 Token + Git 方案。** 只有当用户明确拒绝提供 Token 且文件较小时，才回退到 MCP 工具。

## Token 安全管理（强制）

- **Token 来源**：用户的 GitHub Personal Access Token 存储在飞书文档中备用，使用时从飞书文档获取（具体文档由用户在对话中提供或上下文已知）
- **绝对禁止**：在回复、commit message、日志、文件内容中暴露完整 Token
- **使用方式**：仅在 `git clone` / `git push` 的 URL 中嵌入 Token，如 `https://{owner}:{TOKEN}@github.com/{owner}/{repo}.git`
- **用完清理**：push 完成后立即 `rm -rf` 临时克隆目录（含 Token 的 remote 配置），避免 Token 残留在本地
- **代理备选**：网络受限时可使用 `https://gh-proxy.org/https://github.com/{owner}/{repo}.git`，注意 URL 格式是 `@github.com/` 不是 `@http/github.com/`

## 前置检查

1. 确认 `git` 命令可用（`git --version`）
2. 确认 GitHub Token 可用（从飞书文档获取或用户在对话中提供）
3. 确认仓库存在，分支 `main`
4. 不需要确认 MCP 连接器（纯 git 方案不依赖）

## 第一步：判断方向

根据报告内容判断归属目录。方向分类表见 [references/directions.md](references/directions.md)。

- 匹配现有方向 → 使用对应目录
- 不匹配任何方向 → 创建新目录（英文小写，连字符分隔），并在 README 方向说明表中添加一行
- 报告可能跨多个方向时，选**最主要**的一个方向归档，标签里标注其他方向

**只看文件名/标题判断方向，不需要读取 HTML 文件内容。**

## 第二步：生成规范 HTML

将调研内容转为符合规范的 HTML 文件。

**文件命名**：`YYYY-MM-关键词.html`（日期为报告完成日期，关键词用英文小写连字符）

**HTML 规范**：
- 华为配色浅色系，CSS 变量定义见 [references/html-spec.md](references/html-spec.md)
- 页面结构：标题区（标题+日期+标签）→ 章节正文 → 关键结论 → 数据来源
- 表格表头使用华为红背景白字
- 单文件 HTML，CSS 内联，外部 JS（如 ECharts）用 CDN
- 报告中引用的本地图片需先上传到 `assets/` 目录，再替换为 GitHub raw URL

**如果用户已提供 HTML 文件**：检查是否符合配色和结构规范，不符合则修正后再上传。

## 第三步前置：Push 前 README 审视（强制）

**每次 push 任何内容到仓库前，必须先审视是否需要更新根目录 README。** 不要只 push 文件而忽略 README。

按以下清单判断：

| Push 内容 | 是否需要更新 README | 更新什么 |
|-----------|---------------------|---------|
| 新增/更新调研报告 | ✅ 必须 | 报告索引表（新增行或更新行）+ 底部计数 + 日期 |
| 新增调研方向目录 | ✅ 必须 | 目录结构 + 方向说明表 |
| 新增 Skill / 模板 / 工具 | ✅ 必须 | 目录结构 + 对应说明段落 |
| 删除文件/目录 | ✅ 必须 | 目录结构 + 索引表移除对应行 + 计数 |
| 仅修改已有文件内容、不改变结构 | ⚠️ 视情况 | 如果改变了使用方式或规范，更新对应说明；纯内容修正可不更新 |

**判断口诀**：这次 push 是否改变了仓库的结构、索引、或用户需要知道的信息？是 → 更新 README；否 → 可不更新。

## 第三步：Push 到 GitHub（纯 Token + Git 方案）

### 3.1 委派子 agent（强制）

**大文件上传必须委派子 agent 执行**，主 agent 不碰文件内容，避免污染上下文和浪费 token。

子 agent 任务规范：
- ✅ 只确认文件名 → 判断归类目录 → 执行 git 操作
- ✅ 不需要用 Read 工具读取任何 HTML 文件内容
- ✅ 不需要检查 HTML 内部结构（配色/规范在第二步已确认）
- ❌ 禁止最小化、压缩、修改报告内容
- ❌ 禁止为绕过大小限制而改写文件

### 3.2 Git 标准流程

子 agent 按以下步骤执行（所有操作在临时目录 `/tmp/ai-research-push/` 中进行）：

```bash
# 1. 配置 git 用户
git config --global user.name "{owner}"
git config --global user.email "{owner}@users.noreply.github.com"

# 2. 克隆仓库（带 Token，用完即删）
git clone https://{owner}:{TOKEN}@github.com/{owner}/{repo}.git /tmp/ai-research-push
cd /tmp/ai-research-push

# 3. 复制文件到对应目录（直接 cp，不读内容）
cp "{本地HTML文件路径}" "{方向目录}/{YYYY-MM-关键词}.html"

# 4. 本地更新 README.md（直接编辑文件，不需要 MCP 读 SHA）
#    - 报告索引表最上方插入新行
#    - 底部计数 N → N+1
#    - 底部日期更新为当天

# 5. Commit
git add {方向目录}/ {README.md}
git commit -m "feat: 添加 {报告标题}（{方向}）"

# 6. Push
git push origin main

# 7. 清理（含 Token 的 remote 配置）
rm -rf /tmp/ai-research-push
```

如果有配套图片/资源，同时复制到 `assets/` 目录并一起 commit。

### 3.3 MCP 备选方案（仅小文件 + 用户拒绝提供 Token 时）

使用 `mcp__github_oauth__push_files` 工具：
- `owner`, `repo`, `branch`, `message` 同上
- `files`: 数组，每个元素含 `path` 和 `content`（文件内容字符串）
- **注意**：content 参数受 LLM 输出长度限制（~25K tokens），文件 >50K 字符时完全无法使用，必须换 git 方案

## 第四步：更新 README 索引（强制）

**每篇报告上传后必须更新 README，未更新视为未完成。**

纯 git 方案下，README 在第三步的本地编辑阶段已完成更新并一起 commit。具体要求：

1. 在「报告索引」表**最上方**插入新行（按日期倒序）
2. 字段：`| 日期 | 标题 | 方向 | 标签 | 链接 |`
   - 日期：`YYYY-MM-DD`
   - 标题：报告完整标题
   - 方向：目录名（如 `video-generation`）
   - 标签：`#关键词1 #关键词2`（3-6个）
   - 链接：`[HTML]({相对路径})`
3. 更新底部计数：`已归档报告：N 篇`（N+1）
4. 更新底部日期：`最后更新：YYYY-MM-DD`

如果创建了新方向目录，还需在 README 的「方向说明」表中添加一行。

## 报告更新/刷新（删旧传新）

当用户要求更新、刷新、重新上传已有报告时，**不保留旧版本**，直接替换：

1. **定位旧文件**：从 README 索引表中找到该报告的相对路径
2. **删除旧文件**：在克隆的仓库中 `git rm {旧文件路径}`
3. **上传新文件**：用**新的日期**命名（`YYYY-MM-关键词.html`），`cp` 到对应目录
4. **更新索引行**：在 README 索引表中**更新原有行**的日期和链接，不新增行
   - 日期改为新日期
   - 链接路径改为新文件名
5. **底部计数不变**（替换不是新增），但更新底部日期 `最后更新：YYYY-MM-DD`
6. 一起 `git commit` + `git push`

如果关键词也变了，文件名和索引标题都同步更新。

## 禁止事项（硬约束）

- ❌ **禁止修改报告内容**：禁止为绕过大小限制而最小化 HTML（删空白/注释/压缩）、重写报告、或任何形式的内容改写。文件太大时换 git 通道，不是改文件
- ❌ **禁止暴露 Token**：不在回复、commit message、日志中暴露完整 GitHub Token
- ❌ **禁止主 agent 读大文件**：大文件上传委派子 agent，主 agent 不 Read HTML 内容
- ❌ **禁止跳过 README 更新**：每篇报告归档后必须更新索引表
- ❌ **禁止用 MCP 工具传大文件**：>50K 字符的文件必须用 git 方案

## 质量检查（上传前）

- [ ] **Push 前已审视是否需要更新 README**（结构/索引/说明是否变化）
- [ ] HTML 主色为 `#C7000B`，背景为浅色系
- [ ] 页面有明确的日期标注
- [ ] 有关键结论章节
- [ ] 有数据来源说明
- [ ] 文件名符合 `YYYY-MM-关键词.html`
- [ ] 文件路径在正确的方向目录下
- [ ] README 索引表已更新，链接路径正确
- [ ] 使用纯 Token + Git 方案（非 MCP）
- [ ] 已委派子 agent 执行，主 agent 未读取 HTML 内容
- [ ] Token 未在任何输出中暴露
- [ ] 临时克隆目录已清理

## 常见触发语

**新报告归档：**
- "把这份调研报告归档"
- "上传到 github 仓库"
- "把这个调研存起来"
- "更新报告索引"
- "push 这份报告"
- "归档到 ai-research"

**报告更新/刷新：**
- "刷新这份报告"
- "更新这份调研报告"
- "重新上传报告"
- "把旧的替换掉"
- "报告内容更新了，重新 push"

## 注意事项

- 外部写操作（push、更新 README）不需要额外确认，用户触发本 Skill 即表示授权
- 报告内容来自用户输入或之前的对话，不要编造内容
- 配色规范是硬约束，不要随意更改
- 如果 Token 不可用且文件较大，明确告知用户需要提供 GitHub Token，不要降级到 MCP 工具强行上传（会失败）
