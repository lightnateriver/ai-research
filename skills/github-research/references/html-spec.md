# HTML 报告规范 — 华为配色浅色系

所有调研报告必须使用此规范。CSS 变量定义如下，直接复制到 HTML 的 `<style>` 中。

## CSS 变量

```css
:root {
  --huawei-red: #C7000B;        /* 主色：华为红 */
  --huawei-red-light: #E8384F;  /* 辅助强调 */
  --huawei-red-bg: #FFF0F1;     /* 浅红背景，高亮区块 */
  --text-primary: #1A1A1A;      /* 正文文字 */
  --text-secondary: #555555;    /* 次要文字 */
  --text-muted: #888888;        /* 弱化文字、注释、日期 */
  --bg-page: #F7F8FA;           /* 页面背景 */
  --bg-card: #FFFFFF;            /* 卡片背景 */
  --border-light: #E8E8E8;      /* 边框 */
  --shadow-card: 0 2px 12px rgba(0,0,0,0.06);
  --shadow-hover: 0 4px 20px rgba(199,0,11,0.1);
}
```

## 页面结构

```
<body>
  <header>    标题 + 日期 + 标签（华为红渐变背景，白字）
  <container>
    <section> 调研概述
    <section> 数据/图表（可选，ECharts CDN）
    <section> 详细内容（分章节）
    <section> 横向对比表（可选）
    <section> 关键结论（每篇必须有）
    <section> 数据来源（每篇必须有）
  <footer>    报告日期 + 说明
```

## 组件规范

### 标题区（header）
- 背景：`linear-gradient(135deg, #C7000B 0%, #8B0000 100%)`
- 文字：白色，标题 28px 粗体，副标题 15px
- 元信息：日期、标签、数据来源说明，13px，opacity 0.85

### 章节标题（section-title）
- 左边框 4px 华为红，padding-left 14px
- 字号 20px，粗体

### 卡片（card）
- 白色背景，圆角 12px，padding 28px
- box-shadow: `0 2px 12px rgba(0,0,0,0.06)`
- hover 时阴影变为华为红色调

### 表格
- 表头：华为红背景 `#C7000B`，白字，padding 12px 14px
- 偶数行：`#FAFAFA` 背景
- hover 行：`#FFF0F1` 浅红背景
- 边框：底部 1px `#E8E8E8`

### 标签/徽章（badge）
- 圆角 20px，padding 3px 10px，字号 11px 粗体
- 绿色：`background:#E8F5E9; color:#2E7D32`（支持/有）
- 红色：`background:#FFEBEE; color:#C62828`（不支持/无）
- 橙色：`background:#FFF3E0; color:#E65100`（部分/待观察）
- 蓝色：`background:#E3F2FD; color:#1565C0`（辅助标签）
- 华为红：`background:#FFF0F1; color:#C7000B`（主标签）

### 结论框（conclusion-box）
- 背景：`linear-gradient(135deg, #FFF0F1 0%, #FFFFFF 100%)`
- 边框：1px `#FFCDD2`
- 圆角 12px，padding 24px 28px
- 标题：华为红，16px 粗体

### 信息网格（info-grid）
- CSS Grid，auto-fit minmax(180px, 1fr)
- 每个 item：`#FAFAFA` 背景，左边框 3px 华为红，padding 12px 16px

## ECharts 规范（如使用图表）

- CDN：`https://cdn.jsdelivr.net/npm/echarts@5.4.3/dist/echarts.min.js`
- `backgroundColor: 'transparent'`
- 主色：`#C7000B`，辅助色用同色系渐变
- tooltip 必须包含 `triggerOn:'click'`, `renderMode:'richText'`, `confine:true`
- 图表容器高度：柱状图 420px，雷达图 500px

## 响应式

- 最大宽度 1100px，居中
- 移动端（max-width: 768px）：标题缩小，卡片 padding 减小，表格字号 12px
- 表格外层包 `overflow-x:auto` 防止横向溢出

## 禁止事项

- 禁止使用深色背景（必须浅色系）
- 禁止主色不是 `#C7000B`
- 禁止缺少日期标注
- 禁止缺少关键结论章节
- 禁止缺少数据来源说明
- 禁止使用紫色/靛蓝色系作为主色
