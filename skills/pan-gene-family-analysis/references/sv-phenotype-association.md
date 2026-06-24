# 结构变异检测与变异–表型关联 / SV detection & variant–phenotype association

承接 SKILL.md 第 7、10 步。本文件展开 SV calling 与 PAV/SV/CNV–表型关联(GWAS 思路)的
工具、模型与判读。

## 目录 / Contents
- 1. SV 检测:两条路线
- 2. 变异–表型关联:基因型编码
- 3. 关联模型与群体结构校正
- 4. 显著性、验证与解读

---

## 1. SV 检测:两条路线 / SV calling

聚焦家族成员所在区间的缺失/插入/重复/倒位/易位。

- **路线 A(assembly-based,更准)**:基因组对参考/两两比对 **minimap2** → **SyRI** 鉴定
  SV 与重排(或 MUM&Co、AnchorWave + 调用)。能给出碱基级、解析复杂重排,适合代表性材料。
- **路线 B(read-based,可大群体分型)**:reads 比对后用 **Manta / Delly / Lumpy / Pindel /
  smoove** call SV;**群体层面合并**(如 SURVIVOR / Jasmine)得到统一 SV 集再分型基因型。
  长读长(ONT/PacBio)可用 **Sniffles / cuteSV**,SV 灵敏度更高。
- **过滤**:按支持 reads 数、长度、重复区掩蔽过滤假阳性;家族区间重复性高时尤需谨慎。

**与 PAV 的关系**:一个影响整个基因的缺失型 SV 往往就是某成员 PAV=0 的机制解释;把 SV
与 PAV 对照能互相印证。

## 2. 变异–表型关联:基因型编码 / Genotype encoding

把家族变异编码为可关联的基因型:

- **PAV**:present/absent → **0/1**;
- **CNV**:拷贝数(0,1,2,…)→ 连续或有序基因型;
- **SV**:按 SV 型(有/无该缺失、插入等)→ 0/1 或多态编码;
- **SNP**(如需精细定位):常规 0/1/2。

表型:产量、抗逆(病/旱/盐等)、驯化性状等,连续或二元。

## 3. 关联模型与群体结构校正 / Models

群体关联**必须校正群体结构与亲缘**,否则大量假阳性:

- **全基因组层面 GWAS**(以 PAV/SV 为标记):**混合线性模型 MLM**——固定效应含群体结构
  **Q / PCA**,随机效应含亲缘矩阵 **K**;工具 **GEMMA / GAPIT / EMMAX / TASSEL**。
- **单成员/候选层面**:对某家族成员的 PAV(0/1)分组做**表型差异检验**(t 检验 / Wilcoxon),
  并同样建议在校正群体结构的模型里复核,避免结构混杂。
- **多变异联合**:同一家族多个 SV/PAV 可做基于基因的集合检验(burden test 思路)。

## 4. 显著性、验证与解读 / Significance & validation

- **多重检验校正**:Bonferroni 或 **FDR(BH)**;GWAS 报显著阈值(如 −log10P 阈值)与
  QQ 图/λ(genomic inflation)评估校正是否充分。
- **交叉验证**:把显著关联的成员/SV 与**已知 QTL、驯化/改良选择区间(高 Fst、低多样性
  区)**叠合,一致则证据更强。
- **效应方向**:报告"携带该基因/该 SV 的材料表型更高/抗逆更强"等方向性结论。
- **落点**:显著且方向合理的成员 = 功能与育种重点候选,接下游验证(qRT-PCR、转基因、
  近等基因系等)。

> 🧩【你的经验 · 填这里】你常用的 GWAS 工具与显著性阈值、群体结构校正方式(Q+K vs
> PCA+K)、以及与 QTL/驯化区间交叉验证的具体做法。
