# 顺式作用元件归类:从 PlantCARE 结果到"故事线" / Cis-element Classification

> 用途:第 11 步,把 PlantCARE 预测的启动子顺式元件做功能归类并串联成支撑科学问题的 "故事线"。
> 核心在于**结合具体生物学问题**对预测元件进行功能归类 + 逻辑串联,而不是罗列所有元件。

## 1. 先定"故事线"聚焦方向(归类维度) / Define story-line dimensions

归类前先明确研究要讲的核心故事线。常见维度与代表元件(示例,可持续扩充):

| 故事线维度 | 聚焦内容 | 代表元件(示例) |
|---|---|---|
| **胁迫响应** | 干旱、高/低温、盐碱、病害 | DRE-core、MBS、LTR、STRE、ABRE |
| **激素响应** | ABA、MeJA、生长素、赤霉素 | ABRE、TGACG-motif、ERE、AuxRR-core |
| **生长发育** | 光响应、种子特异表达、分生组织 | G-box、I-box、Box-4、TATA-box |
| **其他特定通路** | 厌氧响应、植物抗病响应等 | [按需补充] |

## 2. 提取与映射元件功能(归类实操) / Extract & map

1. **提取原始功能描述**:从 PlantCARE 输出文件的 **Description 列**提取关键词
   (如 light response、abscisic acid responsiveness、MeJA-responsiveness 等)。
2. **映射至归类维度**:把关键词映射到预设故事线维度。例如
   "abscisic acid responsiveness" → ABA 响应 → 激素响应类;
   "MeJA-responsiveness" → 激素响应类。
3. **合并冗余与标准化**:对同义或功能相近的元件合并(如各类 "myb-binding site" 统一为
   "MYB 结合位点";"light response" 统一为 "光响应")。

## 3. 构建逻辑关联(故事线串联) / Build the narrative

- **按基因/样本归类**:把同一基因(或同一样本)预测到的不同元件,按故事线维度分组统计,
  分析该基因在不同响应通路上的富集情况,解释其在应对特定环境或发育阶段时的潜在调控机制。
- **按比较分析归类**:若涉及不同处理(如正常 vs 干旱胁迫)或不同基因家族,可按故事线
  分类后做**组间对比**,用热图或统计图展示不同故事线下元件的富集差异,以支撑核心科学假设。

## 4. 工具辅助归类(可选) / Tooling

- 数据量较大时:用 **R(如 `tidyverse` 做数据过滤与分组)**,或 **TBtools 的
  "simple plantcare classify"** 功能批量归类,提高效率。
- 出图遵循 SKILL.md 通用原则:终稿优先脚本自定义。

## 注意 / Caveat

归类并非绝对,需根据具体研究背景和生物学意义灵活调整,确保归类后的元件能够有力支撑研究结论。
