# 泛基因组构建路线 / Pan-genome construction routes

承接 SKILL.md「数据来源与两条构建路线」。本文件展开两条路线的工具、步骤、优劣与
决策,供选路与定参数时查阅。

## 目录 / Contents
- 1. 选路决策树
- 2. 路线 A:Assembly-based
- 3. 路线 B:Resequencing-based
- 4. 混合策略
- 5. presence 判定的覆盖度阈值(路线 B)

---

## 1. 选路决策树 / Decision tree

按用户实际持有的数据判断:

- **每份材料都有较完整的基因组组装 + 基因注释** → 走 **路线 A**(信息最全)。
- **只有参考基因组 + 大量材料的二代重测序 reads** → 走 **路线 B**(规模大、成本低)。
- **少数材料能 assembly、其余只有 reads** → **混合策略**(推荐)。
- **物种已有公开 pan-genome 资源(含 PAV / 已发布泛基因组项目)** → 优先**直接复用**,
  省去重头构建(见 `pan-genome-resources.md`)。

关键取舍:**只有 assembly 能发现"参考里没有、仅个别材料特有"的全新基因(novel/private
genes)**;路线 B 只能判断"参考里已有的基因在各材料中的存在与否"。若研究问题关心新基因
的有无,必须至少对代表性材料做 assembly。

## 2. 路线 A:Assembly-based

**输入**:N 份基因组 assembly(fa)+ 基因注释(gff3 / 蛋白 fasta)。

**步骤**:
1. 注释质量统一:各份注释来源不一时,建议用统一流程(如对齐蛋白证据)校正,至少统一
   "只取最长转录本"(沿用 `gene-family-analysis`)。
2. 各份蛋白组 → **OrthoFinder** 做 orthogroup 聚类(见 `orthogroup-and-pav.md`)。
3. 由 orthogroup ↔ 种质对应表生成 PAV(某 orthogroup 在某种质有无成员)与 CNV(成员个数)。
4. 可选:基因组两两/对参考比对(minimap2 + SyRI)做共线性与 SV(见
   `sv-phenotype-association.md`)。

**优点**:可做 CNV、扩张收缩、SV、共线性、发现新基因。**缺点**:每份高质量组装+注释成本高,
注释口径差异会引入噪声。

## 3. 路线 B:Resequencing-based

**输入**:1 份参考基因组 + 注释 + 群体二代重测序 reads(可上百上千份)。

**步骤**:
1. reads 质控(fastp)→ 比对参考(**BWA-MEM** / bowtie2)→ 排序去重(samtools / Picard)。
2. **基于覆盖度的基因 presence/absence**:统计每个家族成员(参考基因区间)在每份材料中的
   **read 覆盖度 / 覆盖比例**,按阈值(见第 5 节)判 present(1)/ absent(0);深度还可
   估**拷贝数(CNV)**。
3. 可选:SV calling(Manta/Delly 等,见 `sv-phenotype-association.md`)与 SNP calling
   (GATK / bcftools)→ 供群体遗传统计与 GWAS。

**优点**:样本量大、成本低,适合群体与表型关联。**缺点**:看不到参考外的新基因;低覆盖/
重复区易误判 absence,需质控。

## 4. 混合策略 / Hybrid (推荐)

少数代表性材料(覆盖物种遗传多样性)做高质量 assembly:用于发现新基因、建非冗余泛基因
集与 SV 图谱;大群体 resequencing:对非冗余泛基因集分型得到 PAV/SV,接表型关联。这是当前
作物泛基因组研究的主流范式,兼顾"发现"与"规模"。

> 🧩【你的经验 · 填这里】你常用的代表性材料挑选标准(覆盖多样性的份数)、非冗余泛基因
> 集的构建工具与去冗余阈值。

## 5. presence 判定的覆盖度阈值(路线 B)

由 read 覆盖判 presence/absence 时需定阈值,**先放可调默认、再据数据校准**:

- 常见做法:某基因 **被 reads 覆盖的碱基比例 ≥ ~50%**(coverage breadth)且平均深度达到
  群体可比水平,判为 present;远低于此判 absent。具体阈值依测序深度、基因长度、重复性调整。
- 务必设**深度下限**(过低深度的样本对该基因判 absent 不可靠,应标记为缺失/不可判,而非 0)。
- 重复区 / 旁系同源高的家族:覆盖度法易混淆,需结合比对唯一性(MAPQ)过滤。

> 🧩【你的经验 · 填这里】你判 presence 的覆盖比例与深度阈值、低质量样本的处理规则、
> 高重复家族的特殊处理。
