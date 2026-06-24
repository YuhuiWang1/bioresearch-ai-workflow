---
name: pan-gene-family-analysis
description: >
  指导对植物/作物在**泛基因组(pan-genome)层面**做基因家族系统分析:从多份种质/
  基因组出发,构建直系同源聚类 (orthogroup) 与存在/缺失变异矩阵 (PAV) → core /
  softcore / dispensable / private 分类与 pan/core 增长曲线 → 拷贝数变异 (CNV) →
  家族扩张/收缩 (CAFE) → 结构变异 (SV) 检测 → 选择压力 (Ka/Ks) 与群体遗传信号
  (π / Tajima's D / Fst) → 泛转录组表达谱 → SV/PAV–表型关联 (GWAS) → 核心保守 vs
  种质特异成员的功能解读。Use this skill whenever the user works on a gene family
  across MULTIPLE genomes/accessions/varieties (not a single genome): pan-genome
  gene family analysis, presence/absence variation (PAV), core/dispensable/private
  genes, orthogroup/OrthoFinder clustering, pan/core curve (open vs closed
  pan-genome, Heaps' law), copy-number variation, gene family expansion/contraction
  (CAFE), structural-variation or PAV based phenotype/GWAS association, or
  pan-transcriptome expression of a family — 包括用户提到"泛基因组""泛基因家族"
  "PAV/存在缺失变异""核心/非核心基因""core/dispensable""泛转录组""种质/品种群体"
  "基因家族扩张收缩"等情形,即使没有明确说出"泛基因家族分析"这几个字,也应使用本技能。
  若用户只分析单一基因组的基因家族,请改用 gene-family-analysis 技能。
---

<!--
  说明(运行时无害,可保留或删除)。本文件有两类标记,分别面向不同的人、不同时机:

  1) 🧩【作者默认 · 填这里】= 给"技能作者(你)"填的,build-time。
     用你自己的科研经验填上默认取值;填好后把引用块改成正式正文。用不到的步骤可整段删。
     搜索 "🧩" 定位所有待你填的项。worked example 与
     references/pan-genome-parameter-anchors.md 等你提供 5–10 篇文献后第二轮补全。

  2) 📄【使用者物种文献 · 运行时】= 给"运行本技能的 AI 助手"看的指令,run-time。
     默认使用者是第一次做、对该物种没有经验,其经验来源是文献;故在这些点上,助手应主动
     邀请使用者提供"该物种"的已发表泛基因组/家族文献,据此把作者默认校准到其具体物种。
     这类块无需作者填写,保留即可(详见正文「经验与默认的两层来源」一节)。
-->

# 泛基因家族分析 Pan Gene Family Analysis

本技能指导你(AI 助手)带领用户,把一个基因家族的分析从"单个参考基因组"扩展到
"一个物种的多份种质 / 多个近缘物种"的**泛基因组层面**,讲清"哪些成员人人都有
(核心、保守)、哪些成员只有部分材料才有(可变、可能与适应/驯化/抗逆相关)",
并把这些变异与表型联系起来,挖掘关键功能基因。

## 通用原则 / Guiding principles

以下原则贯穿所有步骤,无需每步重复判断:

1. **可复现优先**:每一步记录工具版本、数据库版本、参考基因组版本、关键参数与阈值,
   让结果能被他人(和审稿人)重做。泛基因组分析样本多、流程长,可复现性尤其关键。
2. **终稿图表优先用脚本自定义**:正式发表的图优先用 **Python / R(ggplot2 / ggtree /
   pheatmap 等)脚本自定义绘制**,保证质量与辨识度;GUI 工具仅用于快速草图或无脚本
   权限时兜底,终稿建议脚本重绘。
3. **泛基因组分析基本依赖命令行 / HPC**:OrthoFinder、CAFE、SV calling、群体遗传统计
   等几乎都需要命令行与较大算力,**GUI 兜底空间有限**。开工前先确认用户的计算环境
   (本机 / 服务器 / 集群、核数、内存),据此决定样本规模与工具选择。
4. **阈值一次定、全文一致**:core/softcore/dispensable/private 的判定阈值(出现在多少
   比例的种质里)一旦选定,必须贯穿 PAV 分类、增长曲线、表型关联全程,不可中途更换。
5. **坐标与命名统一到参考基因组**:所有 PAV、SV、坐标、命名都以选定的参考基因组为锚,
   不同种质的成员要能映射回参考坐标系,否则跨样本无法比较。

## 与单基因组基因家族技能的关系 / Relationship to gene-family-analysis

本技能是 `gene-family-analysis`(单基因组版)的**泛基因组扩展**,二者分工:

- **与单基因组重合、且应复用**的步骤——单份基因组内的**家族成员鉴定**(HMMER/BLAST +
  结构域核对)、理化性质、亚细胞定位、系统发育建树、基因结构/保守基序/结构域、单基因组
  内的染色体定位与共线性、顺式元件——**沿用 `gene-family-analysis` 的标准做法**,本技能
  只做必要简述并**指向该技能**,不重复展开(尤其是阈值:E-value 先宽松后过滤、核心结构域
  覆盖 > 30%、只保留最长转录本等,均沿用)。
- **本技能新增、单基因组没有**的步骤——orthogroup 聚类、PAV 矩阵、core/dispensable 分类、
  pan/core 曲线、CNV、CAFE 扩张收缩、SV 检测、群体选择信号、泛转录组表达、PAV/SV–表型
  关联——是本技能的主体,下文详述。

实操上常见两层结构:**先对每份基因组按单基因组技能做成员鉴定 → 再把各份结果汇入本技能
的泛层流程**。

## 经验与默认的两层来源 / Two tiers of defaults

本技能的参数与判断有两层来源,助手运行时按此协作——这也对应正文里两类标记:

1. **作者默认(build-time,对应 `🧩【作者默认】`)**:由技能作者依自身科研经验、以及据多篇
   高质量文献整理的 `references/pan-genome-parameter-anchors.md`,给出**通用默认取值**。
   作者尚未填写时,助手沿用各 references 里的通用默认,并明确告知使用者"此为通用默认、可调"。
2. **使用者的本物种文献(run-time,对应 `📄【使用者物种文献】`)**:**默认使用者是第一次做、
   对该物种没有经验,其经验来源是文献**。因此在带此标记的决策点,助手应**主动邀请使用者
   提供"该物种"已发表的泛基因组 / 家族文献(DOI 或 PDF)**,从中提炼对应参数——**用自己的
   话归纳、标注出处(物种 / 期刊 / 年份)、绝不照搬原文(版权)**——据此把作者默认**校准到
   使用者的具体物种**。使用者无文献时,沿用作者默认并说明其适用范围。

一句话:**作者经验 / 文献 → 通用默认;使用者的本物种文献 → 运行时校准到具体物种。**
两者不冲突——本物种文献优先于通用默认,缺失时回退到通用默认。

## 何时使用 / When to apply

当用户在**多份基因组/种质/物种**上分析基因家族时启动本流程:构建泛基因组的家族
PAV、划分核心/非核心成员、画 pan/core 曲线、做 CNV 或 CAFE 扩张收缩、SV 检测、群体
选择信号、泛转录组表达、或把 PAV/SV 与表型做关联。若用户只问其中一步,直接跳到对应
小节,但仍提醒前置依赖(如"做 PAV 关联前必须先有可靠的 orthogroup 与 PAV 矩阵")。
**只涉及单一基因组**时改用 `gene-family-analysis`。

## 语言 / Language

默认使用**用户提问所用的语言**作答与输出;用户明确要求某种语言时以其为准。无论中英文
输出,基因名、工具名、数据库名、统计量一律**保留英文**(如 OrthoFinder、PAV、CAFE、
Ka/Ks、Fst、Tajima's D、Heaps' law、SyRI、GWAS)。

Respond in the **language of the user's request** by default; keep technical terms
(gene/tool/database names, metrics) in English regardless of output language.

## 开始前必须确认的输入 / Scoping

动手前先与用户确认以下信息;缺失项主动追问,不要默认:

1. **泛基因组的"泛"指什么范围**:
   - **种内(intraspecific)**——同一物种的多份种质/品种/材料(PAV、core/dispensable 是
     核心,是默认主线);
   - **种间(interspecific)**——多个近缘物种(偏 orthogroup + CAFE 扩张收缩,是可切换分支)。
2. **样本规模与来源**:多少份种质/基因组?来源是?(典型:**公共数据库下载的基因组
   assembly** + **本地实验室的二代重测序数据**;多数用户**没有**现成的 orthogroup/PAV 矩阵,
   需从零构建。)
3. **数据形态决定构建路线(见下一节)**:是 **assembly-based**(每份都有较完整的基因组+
   注释)还是 **resequencing-based**(只有 reads,需比对到参考基因组)?二者流程差别大。
4. **参考基因组(reference)**:选哪一份做坐标与命名的锚?优先 T2T / 高质量长读长组装。
5. **家族定义依据**:家族名称 + Pfam 结构域编号 / HMM 模型(同 `gene-family-analysis`)。
6. **core/dispensable 阈值**:出现在多少比例种质里算 core / softcore / dispensable / private?
   有惯用标准就用,没有先用本技能默认(见 references)并向用户标注可调。
7. **是否有表型/群体信息**:有无表型数据(产量、抗逆、驯化性状)与亚群划分?决定能否做
   PAV/SV–表型关联。
8. **计算资源**:本机 / 服务器 / 集群,核数与内存——决定样本规模与工具(原则 3)。

**参考基因组与数据库选择**:沿用 `gene-family-analysis` 的规则——优先物种专用数据库、
优先 T2T / 长读长最新组装、记录版本与访问日期。泛基因组层面再补充:**优先使用已发布的
物种泛基因组资源**(若该物种已有 pan-genome 数据库 / 已发表泛基因组项目),可直接复用其
种质集合与 PAV,省去重头构建。相关资源整理见 `references/pan-genome-resources.md`。

---

## 数据来源与两条构建路线 / Data sources & two construction routes

多数用户没有现成矩阵,需要从原始数据构建。**先判断走哪条路线**,这是泛层分析的起点。
两条路线的工具、步骤、优劣与决策树详见 **`references/pan-genome-construction.md`**,
此处只给概览:

- **路线 A — Assembly-based(多份基因组组装 + 注释)**:每份种质都有较完整的基因组与
  基因注释。直接对各份蛋白组做 orthogroup 聚类(OrthoFinder),由"某 orthogroup 在某
  种质中是否有成员"得到家族的 PAV。**信息最全**(可做 CNV、扩张收缩、SV、共线性),
  但要求每份都有高质量组装与注释,成本高。
- **路线 B — Resequencing-based(参考基因组 + 群体二代重测序)**:多数材料只有 reads。
  把 reads 比对到参考基因组,**基于覆盖度判断每个家族成员在每份材料中是否"存在"**
  (presence/absence by read coverage),或用 reference-based SV/PAV calling 得到矩阵。
  **样本量可以很大、成本低**,适合群体与表型关联;但只能看"参考里有的基因在各材料中的
  存在与否",**看不到参考里没有、仅个别材料特有的全新基因**(需 assembly 才能发现)。

**混合策略(推荐且常见)**:少数代表性材料做高质量 assembly(发现新基因、建 SV 图谱),
大群体用 resequencing 做 PAV/SV 分型并接表型关联——兼顾"发现"与"规模"。

---

## 工作流程总览 / Workflow

按以下顺序推进;每步先说"目的",再给"标准做法 + 关键参数",最后留经验补充。

### 0. 模式与参考确定 Mode & reference selection

**目的**:锁定"种内 vs 种间"模式、参考基因组、种质集合、core/dispensable 阈值与构建
路线——后续一切以此为锚。

**标准做法**:确认 Scoping 全部 8 项;固定参考基因组版本;固定阈值;在 Methods 里写清
种质清单(含来源、组装/注释版本、访问日期)与路线选择理由。

### 1. 跨种质的家族成员鉴定 Per-genome member identification

**目的**:在每份基因组中得到可靠的家族成员清单,作为 orthogroup/PAV 的输入。

**标准做法**:
- **路线 A**:对**每份种质的蛋白组**分别按 `gene-family-analysis` 的成员鉴定流程
  (HMMER `hmmsearch` + 模式物种 BLASTp 反向补漏 + 结构域逐条核对)鉴定成员;沿用其
  全部阈值(E-value 先宽松后过滤、核心结构域覆盖 > 30%、只保留最长转录本、可疑 gene
  model 人工复核)。
- **路线 B**:以参考基因组的家族成员为锚,把各材料 reads 比对到参考,用覆盖度/基因型
  判断每个成员的存在与否(详见 `references/pan-genome-construction.md`)。

**关键点**:跨种质鉴定**标准必须统一**——同一 HMM、同一阈值、同一结构域核对规则用于所有
种质,否则 PAV 会被"鉴定口径不一致"污染。

### 2. 直系同源聚类 Orthogroup clustering

**目的**:把分散在各种质中的家族成员**归并成跨种质可比的"泛家族单元"(orthogroup)**,
这是 PAV 矩阵的行定义。

**标准做法**:
- 用 **OrthoFinder**(默认,蛋白组)对全部种质做 orthogroup 推断;或 OrthoMCL / DIAMOND
  + MCL。家族层面可只取含家族成员的 orthogroups。
- 输出:orthogroup ↔ 各种质成员的对应表(后续 PAV、CNV、扩张收缩都基于它)。

**关键参数与判读见 `references/orthogroup-and-pav.md`**(OrthoFinder 序列搜索引擎与
inflation 参数、如何从 orthogroup 表生成 PAV、单拷贝直系同源用于建种质树等)。先查阅
该文件再操作。

### 3. PAV 矩阵构建 Presence/Absence Variation matrix

**目的**:得到核心数据结构——**行 = 家族成员/orthogroup,列 = 种质,值 = 存在(1)/缺失(0)
**(CNV 时存拷贝数),所有泛层结论都由它推导。

**标准做法**:
- 路线 A 由 orthogroup 表生成;路线 B 由覆盖度/基因型生成(设定 presence 的覆盖度阈值,
  如覆盖度 ≥ 某比例判为 present——阈值见 references)。
- 质控:剔除比对/注释质量差导致的假缺失;必要时对边界 case 人工核查。

详见 `references/orthogroup-and-pav.md`。

### 4. 核心/非核心分类与 pan/core 曲线 Core/dispensable & pan/core curve

**目的**:把成员分为 **core(几乎人人都有)/ softcore(近核心)/ dispensable(可变,部分
材料有)/ private(仅个别材料特有)**,并判断泛基因组是 **开放(open)还是封闭(closed)**。

**标准做法**:
- **分类**:按出现频率分箱。常见默认(**可调,先与用户确认**):存在于 **100% 种质 = core**、
  **≥ 90%(或除 1 份外) = softcore**、**1 份 < x < softcore 阈值 = dispensable**、**仅 1 份 =
  private/unique**。家族成员少时按比例分箱需谨慎,样本数太少时分类不稳。
- **pan/core 增长曲线**:逐步加入种质,统计 pan-size(并集)与 core-size(交集)的变化,
  **多次随机抽样取均值**画 rarefaction 曲线;用 **Heaps' law** 等模型拟合,由指数判断
  pan-genome 开放(还在增长)还是封闭(趋于饱和)。
- 出图:core/dispensable 堆叠比例图 + pan/core 曲线,脚本绘制(R/Python)。

分箱阈值、曲线拟合模型、抽样次数等见 `references/orthogroup-and-pav.md`。

> 🧩【作者默认 · 填这里】你实验室惯用的 core/softcore/dispensable/private 阈值、最少需要
> 多少份种质才认为分类可靠、pan/core 曲线的拟合模型与抽样次数偏好。
>
> 📄【使用者物种文献 · 运行时】core/dispensable 阈值与 pan-genome 开放性**因物种、种质规模、
> 群体结构而异**。若使用者第一次做该物种:邀其提供该物种(或近缘物种)的泛基因组文献,
> 提炼其采用的分箱阈值、最少种质数、曲线拟合与开放/封闭结论(归纳+标注出处,不照搬),
> 据此校准上面的作者默认;无文献则用作者默认并说明其为通用值、需按样本量谨慎。

### 5. 拷贝数变异 Copy number variation (CNV)

**目的**:在"有/无"之外,刻画家族成员在不同种质间的**拷贝数差异**——拷贝数扩增常与
剂量效应、抗逆、驯化相关。

**标准做法**:由 orthogroup 表(路线 A,数每种质成员数)或 read-depth(路线 B,按深度
估拷贝数)得到 CNV 矩阵;按种质/亚群比较,定位高变拷贝的成员;热图展示。

> 🧩【作者默认 · 可选补充】你判定"显著拷贝数变异"的标准,以及 read-depth 估拷贝数时的
> 归一化方式(用默认可删本行)。
>
> 📄【使用者物种文献 · 运行时】若使用者想贴合自己物种:可邀其提供该物种关于家族拷贝数
> 变异的文献,提炼其显著 CNV 判定与归一化做法(归纳+标注出处,不照搬)以校准默认。

### 6. 家族扩张/收缩 Gene family expansion & contraction

**目的**:在**种间或亚群**层面,沿系统发育树定量哪些分支发生了家族的扩张/收缩,推断
进化动力。**主要用于种间模式;种内多份种质亦可按亚群做。**

**标准做法**:
- 以 orthogroup 的基因计数矩阵 + 物种/亚群**超度量树(ultrametric tree)** 作输入,运行
  **CAFE**(估计出生-死亡率 λ,标出显著扩张/收缩的分支与家族)。
- 结合家族功能解读扩张/收缩的生物学含义(如抗病基因家族在某支扩张)。

CAFE 输入准备、λ 设定、显著性判读见 `references/selection-and-popgen.md`。

### 7. 结构变异检测 Structural variation (SV) detection

**目的**:刻画家族区域的 SV(缺失/插入/重复/倒位/易位),为 PAV 提供机制解释,并作为
后续表型关联的变异信号之一。

**标准做法**:
- 路线 A:基因组两两/对参考比对(**minimap2 + SyRI**,或 MUM&Co)鉴定 SV。
- 路线 B:reads 比对后用 **Manta / Delly / Lumpy / Pindel** 等 call SV;群体层面合并基因型。
- 聚焦家族成员所在区间,统计影响家族的 SV 类型与频率。

SV calling 工具选择、过滤、群体分型见 `references/sv-phenotype-association.md`。

### 8. 选择压力与群体遗传信号 Selection & population genetics

**目的**:评估家族成员承受的自然选择(正选择/纯化选择),识别受选择的关键成员。

**标准做法**:
- **Ka/Ks(dN/dS)**:对家族内/跨种质的同源基因对或各进化分支计算 Ka、Ks 及比值,
  判断纯化选择(< 1)/中性(≈ 1)/正选择(> 1);可做分支模型/位点模型(PAML codeml)
  识别受正选择的位点。**承接 `gene-family-analysis` 中复制基因对 Ka/Ks 的做法,在泛层
  扩展到多分支/多样本。**
- **群体遗传统计**(种内、需群体变异数据):核苷酸多样性 **π**、**Tajima's D**(偏离中性)、
  群体分化 **Fst**(亚群/驯化前后),定位家族区域的选择信号(选择清除 selective sweep)。

方法、工具(PAML、VCFtools、pixy 等)与参数见 `references/selection-and-popgen.md`。

> 🧩【作者默认 · 填这里】你判定正选择/选择清除的阈值与多重检验校正习惯,以及种内做
> Ka/Ks vs 群体统计的取舍。
>
> 📄【使用者物种文献 · 运行时】选择信号的窗口大小、经验分位阈值随物种群体规模与多样性
> 而异。若使用者第一次做该物种:邀其提供该物种的群体遗传/选择分析文献,提炼其窗口、
> 阈值与校正做法(归纳+标注出处,不照搬)以校准默认。

### 9. 泛转录组表达谱 Pan-transcriptome expression

**目的**:结合**泛转录组**数据,分析家族成员在不同**组织 / 发育阶段 / 处理(如胁迫响应)
**中的表达差异,评估功能分化;并把表达与 PAV/拷贝数联系(如 private 成员是否仅在特定
胁迫下表达)。

**标准做法**:
- 数据来源:优先物种泛基因组/专用数据库收录的多材料 RNA-seq(承接 `gene-family-analysis`
  的取数原则);跨材料表达需注意把表达量映射到统一的 orthogroup/参考成员上。
- 标准化 **TPM/FPKM**;**Z-score** 变换后 **R/pheatmap** 热图 + 聚类;比较组织/处理/材料
  间的特异性表达,并标注该成员的 core/dispensable 身份。
- 重点候选:在特定处理/组织中**特异表达**、或**仅在部分材料中存在且响应胁迫**的成员。

跨材料表达整合的坑与做法见 `references/pan-transcriptome-expression.md`。

### 10. SV/PAV–表型关联分析 Variant–phenotype association

**目的**:把家族的**变异信号(SV、PAV、CNV)与表型(产量、抗逆、驯化性状等)关联**,
挖掘关键功能基因——这是泛基因组分析最有价值的落点之一。

**标准做法**:
- 以 PAV/SV/CNV 作为基因型(0/1 或拷贝数),表型作因变量,在群体中做**关联分析**:
  - PAV/SV 作为标记的 **GWAS**(混合线性模型 MLM,校正群体结构 Q + 亲缘 K,如
    GEMMA / GAPIT / EMMAX);或对单个家族成员的 PAV 做**基因型-表型差异检验**
    (t 检验 / 非参检验 + 多重检验校正)。
  - 与已知驯化/改良选择区间、QTL 叠合,增强证据。
- 输出:显著关联的成员 + 效应方向(有该基因 → 表型更高/抗逆更强等),作为功能候选。

群体结构校正、模型选择、显著性阈值见 `references/sv-phenotype-association.md`。

> 🧩【作者默认 · 填这里】你做 PAV/SV 关联时的群体结构校正、显著性阈值(Bonferroni /
> FDR)、以及如何与 QTL/驯化区间交叉验证的习惯。
>
> 📄【使用者物种文献 · 运行时】关联模型与显著性阈值依群体结构与 marker 数而异,且能否与
> QTL/驯化区间交叉验证取决于该物种已有研究。若使用者第一次做该物种:邀其提供该物种的
> GWAS/驯化/QTL 文献,提炼其模型、阈值与已知性状区间(归纳+标注出处,不照搬)以校准默认。

### 11. 核心保守 vs 种质特异成员的功能解读 Integration & interpretation

**目的**:把以上结果整合成一个连贯的进化-功能故事,区分两类成员并给出功能假设:

- **core / 高度保守、纯化选择**的成员:多为家族的"看家"功能,功能保守;
- **dispensable / private、受正选择或拷贝数高变、与表型关联**的成员:更可能与**适应、
  驯化、抗逆**等性状分化相关,是功能研究与育种的重点候选。

**标准做法**:综合 PAV 身份 + CNV + 选择信号 + 表达特异性 + 表型关联,给每个重点候选写一
句"它为什么值得做下游验证";关键候选可接 qRT-PCR / 转基因 / AlphaFold 结构等(承接
`gene-family-analysis` 的延伸分析)。

---

## 结果整合与作图规范 / Integration & figure standards

**目的**:把分散结果整合成期刊级图表,讲清"核心 vs 可变 + 变异如何影响表型"的故事。

**标准做法**:常见主图——PAV 矩阵热图(按 core/dispensable 排序)+ pan/core 曲线 +
core/dispensable 比例图 + 关键成员的 SV/PAV–表型关联图 + 跨材料表达热图。全文统一配色与
命名(命名沿用 `gene-family-analysis` 规则),每图注明数据与方法、样本数。

> 🧩【作者默认 · 填这里】你的泛基因组家族文章"必备图表清单"、组合图排版与配色规范、
> 字体/分辨率要求。
>
> 📄【使用者物种文献 · 运行时】若使用者无固定图表习惯:可邀其提供该物种 1–2 篇高质量泛
> 基因组家族文章作范例,参照其主图构成与呈现方式(学习排版思路,不复制具体图件)。

## 可复现性清单 / Reproducibility checklist

完成后核对并向用户报告(便于写 Methods 与回应审稿):

- [ ] 种质清单:每份的来源、组装版本、注释版本、访问日期
- [ ] 参考基因组版本;构建路线(A / B / 混合)
- [ ] 每个工具的名称与**版本号**(OrthoFinder、CAFE、SV caller、PAML、GWAS 工具等)
- [ ] 成员鉴定阈值(沿用单基因组技能)与跨种质统一口径的说明
- [ ] core/softcore/dispensable/private 的**判定阈值**与 pan/core 曲线抽样/拟合设置
- [ ] PAV/SV presence 判定的覆盖度/质量阈值
- [ ] 选择分析与表型关联的模型、群体结构校正、显著性阈值与多重检验校正
- [ ] 所有自定义参数与脚本(建议附 GitHub 链接)

> 🧩【作者默认 · 填这里】你在泛基因组 Methods 里固定会写、但新手常漏掉的细节。
>
> 📄【使用者物种文献 · 运行时】该物种已发表泛基因组文章的 Methods 是第一次做者最好的
> 清单来源:可邀使用者提供,核对其报告了哪些参数/版本/阈值,据此补全本物种的可复现项。

---

## 输出格式约定 / Output format

除非用户另有要求,按此结构组织交付:

```
# [物种/类群] [家族名] 泛基因家族分析
## 1. 材料与构建路线(种质清单、参考、路线 A/B、阈值)
## 2. orthogroup 聚类与 PAV 矩阵
## 3. 核心/非核心分类与 pan/core 曲线(开放 vs 封闭)
## 4. 拷贝数变异 / 家族扩张收缩(CNV / CAFE)
## 5. 结构变异(SV)与选择压力(Ka/Ks、π、Tajima's D、Fst)
## 6. 泛转录组表达谱
## 7. SV/PAV–表型关联与关键候选基因
## 8. 核心保守 vs 种质特异成员的功能解读与小结
## 方法与可复现性信息
```

## 一个填写示例 / Worked example(第二轮据文献补全)

> 🧩【作者默认 · 待补充,依赖你提供的文献】将用你筛选的 5–10 篇高质量植物泛基因组家族
> 文章,提炼成一个完整 worked example:从"物种 + 家族 + 种质规模"开始,把每步实际用的
> 工具、参数、阈值、得到的 core/dispensable 比例、pan-genome 开放性结论、关键表型关联,
> 浓缩成 15–25 行;并据这些文献填好
> `references/pan-genome-parameter-anchors.md` 的默认参数表。
>
> 📄【使用者物种文献 · 运行时】此处的作者示例用于"跨物种通用示范";运行时若使用者做的是
> 另一物种,助手应额外参照使用者提供的该物种文献来取参,而非照搬本示例的物种特异取值。
