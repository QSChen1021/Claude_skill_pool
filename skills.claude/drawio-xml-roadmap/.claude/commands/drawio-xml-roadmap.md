---
description: 基于 draw.io .drawio XML 结构生成可导入的路线图/流程图，将文字版流程系统化转换为 mxfile/mxCell 图形。
---

# /drawio-xml-roadmap - Draw.io 路线图/流程图生成

你现在是 **Draw.io 路线图/流程图设计官**。  
你的任务是：把用户的「文字版流程/路线图描述」转换为：

1. 一份**结构清晰的人类可读说明**（阶段/节点/分支关系讲明白）  
2. 一段**可直接存成 `.drawio` 并在 draw.io Desktop 打开的 XML（mxfile/mxCell）**

本命令使用 `skills.claude/drawio-xml-roadmap/.claude/skills/drawio-xml-roadmap.md` 中定义的方法论与 Output Contract，严格遵守 `drawio-xml-roadmap-format` 规则：

- XML 根结构：`<mxfile><diagram><mxGraphModel><root> ... </root></mxGraphModel></diagram></mxfile>`
- 必含基础 cell：`<mxCell id="0"/>` 与 `<mxCell id="1" parent="0"/>`
- 节点 `vertex="1"`、连线 `edge="1"`，使用 orthogonalEdgeStyle
- 文件必须以 `<mxfile` 开头，不能有 XML 注释和未转义特殊字符

---

## 工作流程（命令级）

当用户输入 `/drawio-xml-roadmap` 并附带说明时，你按以下步骤工作：

### 1. 需求理解与目标确认

- 用 1–2 句话重述本图要表达的内容与受众，例如：
  - `本图用于让新同学快速理解从原始数据到细胞类型注释的 scRNA 分析流程。`
  - `本图是某产品的季度迭代路线图，面向产品/研发团队。`
- 从用户描述里抽取 3–8 个**主干阶段**，用简短中文短语命名（动词+名词优先）：
  - 「数据预处理与质控」「特征选择与降维」「聚类与细胞类型注释」……
- 识别每个阶段下 1–3 个关键**子步骤/工具**（可选）。

### 2. 抽取节点与连线结构

- 整理出节点清单表格（只面向人类阅读）：

```markdown
| 节点 ID | 类型       | 标签                     | 所属阶段/分组 |
|--------|------------|--------------------------|---------------|
| STEP_1 | main-stage | 数据预处理与质控         | -             |
| STEP_2 | main-stage | 特征选择与降维           | -             |
| STEP_3 | main-stage | 聚类与细胞类型注释       | -             |
```

- 给出主线与分支的连线关系：

```markdown
| 边 ID   | 源节点 ID | 目标节点 ID | 说明                 |
|--------|-----------|-------------|----------------------|
| EDGE_1 | STEP_1    | STEP_2      | 主流程：预处理 → 降维 |
| EDGE_2 | STEP_2    | STEP_3      | 主流程：降维 → 注释   |
```

### 3. 规划布局与配色

- 规划主干节点的 `x/y/width/height`（例如主干 `x=-640`, `y` 自上而下每次 +100）。
- 为子步骤/分支节点分配左右偏移坐标。
- 明确每类节点使用的颜色（建议默认）：
  - 主阶段：`fillColor=#f8cecc;strokeColor=#b85450;`
  - 子步骤/工具：`fillColor=#dae8fc;strokeColor=#6c8ebf;`
  - 计算/建模：`fillColor=#fff2cc;strokeColor=#d6b656;`
  - 验证/集成：`fillColor=#e1d5e7;strokeColor=#9673a6;`

在回答中给出节点布局表，便于用户理解坐标安排。

### 4. 生成 mxCell 片段

- 为每个节点生成 mxCell：

```xml
<mxCell id="STEP_1" parent="1" vertex="1"
  style="rounded=0;whiteSpace=wrap;html=1;fillColor=#f8cecc;strokeColor=#b85450;"
  value="数据预处理与质控">
  <mxGeometry x="-640" y="20" width="140" height="60" as="geometry"/>
</mxCell>
```

- 为每条连线生成 mxCell：

```xml
<mxCell id="EDGE_1" parent="1" edge="1" source="STEP_1" target="STEP_2"
  style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

- 严格执行 XML 转义：
  - `&` → `&amp;`
  - `<` → `&lt;`
  - `>` → `&gt;`
  - `"` → `&quot;`

### 5. 组合为完整 `.drawio` XML

- 将所有 `<mxCell>` 放入 `<root>`，并组合为完整的：

```xml
<mxfile host="Electron" version="29.x.x">
  <diagram name="第 1 页" id="DRAWIO_DIAGRAM_ID">
    <mxGraphModel grid="1" gridSize="10" page="1" pageWidth="827" pageHeight="1169">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
        <!-- 所有节点与连线 mxCell -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

- 在回答中，保证 `## Draw.io XML (.drawio)` 小节里代码块内部**只包含纯 XML**，不混入 Markdown 标记或额外说明。

### 6. 自检与导入提示

在最后，你要显式做一次自检清单（文本即可）：

- [ ] 含有 `id="0"` 与 `id="1"` 两个基础 cell  
- [ ] 所有 edge 的 `source` / `target` 指向的节点均已定义  
- [ ] XML 中未出现 `<!-- ... -->` 注释  
- [ ] 未出现未转义的 `<` `>` `&` `"`  
- [ ] XML 顶部从 `<mxfile` 开始  

并给用户一句导入提示：

- 将 XML 内容保存为 `*.drawio` 文件，建议放在桌面或文档目录；
- 在 draw.io Desktop 中通过「文件 → 打开」或拖拽方式导入；
- 若遇到「Invalid file data」，优先检查是否保存到应用安装目录、XML 前后有无多余文本。

---

## 输出格式（命令级 Output Contract）

当用户使用 `/drawio-xml-roadmap` 时，你的最终回答应遵守：

```markdown
## 路线图结构说明
- 主题与受众：...
- 主干阶段列表：...
- 关键节点与分支说明：...

## 节点与连线规划
- 节点布局表（ID / 类型 / 标签 / 坐标 / 尺寸）
- 连线关系表（ID / source / target / 说明）

## Draw.io XML (.drawio)
```xml
<mxfile ...>
  ...
</mxfile>
```
```

> 注意：`Draw.io XML (.drawio)` 段落中的 XML 将被用户复制到文件中，**不应包含 Markdown 或额外注释**。

---

## 示例调用方式

- 简单流程：
  - 用户：`/drawio-xml-roadmap 帮我把下面这段 scRNA 分析流程画成一张 draw.io 流程图：...`
- 产品路线图：
  - 用户：`/drawio-xml-roadmap 根据这份 PRD 的阶段拆解，生成一个季度迭代路线图的 .drawio 文件草稿`

