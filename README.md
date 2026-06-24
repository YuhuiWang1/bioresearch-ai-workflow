# bioresearch-ai-workflow

**AI-assisted, reproducible workflows for plant / crop bioinformatics — from gene-family analysis to manuscript review.**
**面向植物 / 作物生物信息学的「可复现」AI 辅助工作流 —— 从基因家族分析到论文审阅。**

> A curated collection of **Claude Skills** for end-to-end bioinformatics analysis, plus a set of **experience-based prompts** for proofreading and reviewing the resulting manuscripts. Built around one principle: *every result should be reproducible by a reviewer.*
>
> 一套用于端到端生信分析的 **Claude Skills**,外加一组基于作者亲身科研经验、用于论文**写作后校对与审阅**的 **prompts**。核心理念只有一条:*每一个结果,审稿人都能照着重做出来。*

---

## What is this? / 这是什么

`bioresearch-ai-workflow` packages hard-won, real-world research experience into structured, machine-readable **skills and prompts** that an AI assistant (e.g. Claude) can follow to:

1. **Run bioinformatics analyses** — currently focused on **plant / crop gene-family analysis** at both the single-genome and the pan-genome level.
2. **Review and polish the manuscript** — experience-based prompts for the **proofreading / self-review / peer-review-style critique** stage that comes *after* the analysis is written up.

本仓库把作者真实的科研经验,沉淀成结构化、可被 AI 助手(如 Claude)直接调用的 **skill 与 prompt**,覆盖两件事:**①跑分析**(目前聚焦植物/作物**基因家族分析**,含单基因组与泛基因组两个层级);**②审稿子**(论文初稿完成后的**校对、自审、模拟同行评审**)。

These are not black-box pipelines. Each skill encodes *decisions* — which database to trust, which threshold to use, when a gene model is suspect, when a figure should be redrawn — the kind of judgment normally locked in a senior researcher's head.

它们不是黑箱流水线,而是把「该信哪个数据库、阈值怎么定、什么时候 gene model 可疑、什么时候图该重画」这类通常只存在于资深研究者脑子里的**判断**写了下来。

---

## Repository structure / 仓库结构

```
bioresearch-ai-workflow/
├── skills/                              # Bioinformatics analysis skills / 分析类技能
│   ├── gene-family-analysis/            # Single-genome gene family / 单基因组基因家族
│   │   └── SKILL.md
│   └── pan-gene-family-analysis/        # Pan-genome level / 泛基因组层面
│       └── SKILL.md
├── prompts/                             # Manuscript proofreading & review / 论文校对与审阅
│   └── ...                              # Experience-based review prompts / 基于经验的审稿 prompt
└── README.md
```

> Adjust the paths above to match your actual layout. 请按你仓库的真实目录结构微调上面的路径。

---

## Skills / 技能

### 1. `gene-family-analysis` — Single-genome gene family analysis / 单基因组基因家族分析

A complete, reproducible workflow that takes a gene family from **"identify the members"** all the way to **"tell a biological story."**
从「鉴定成员」一路做到「讲出生物学故事」的完整可复现流程。

**Covered steps / 覆盖步骤:**

- **Member identification / 家族成员鉴定** — `HMMER` (`hmmsearch`) + reciprocal `BLASTp`, with per-protein domain verification (NCBI-CDD / Pfam / SMART / InterPro). Includes the *lenient-E-value-then-filter* strategy, the *core-domain coverage > 30%* keep-rule, suspicious-gene-model review, and *longest-isoform-only* handling.
- **Sequence curation & naming / 序列整理与命名** — Arabidopsis-aligned naming conventions for cross-literature consistency.
- **Physicochemical properties / 理化性质** — `ExPASy ProtParam` (length, MW, pI, instability, GRAVY).
- **Subcellular localization / 亚细胞定位** — `WoLF PSORT` / `Plant-mPLoc` / `DeepLoc` with cross-validation.
- **Phylogenetic analysis / 系统发育分析** — `MAFFT` → `trimAl` → `IQ-TREE` (ModelFinder, ML) and/or `MEGA` (NJ); bootstrap ≥ 1000; reference taxa for subfamily definition.
- **Gene structure & conserved motifs / 基因结构与保守基序** — exon–intron from GFF3; `MEME` Suite motifs; conserved domains.
- **Chromosomal localization & gene clusters / 染色体定位与基因簇** — physical-distance window + gene-density rules.
- **Gene duplication & synteny / 基因复制与共线性** — `MCScanX` (tandem / segmental / WGD), intra- & inter-species synteny, **`Ka/Ks`** selection pressure.
- **Cis-acting regulatory elements / 顺式作用元件** — ~2000 bp promoter → `PlantCARE`, functional classification into a story line.
- **Expression profiling / 表达模式分析** — `FPKM`/`RPKM`/`TPM`, `R` / `pheatmap` heatmaps + clustering, candidate selection.
- **Optional extensions / 延伸分析** — `AlphaFold` structure, `miRNA` targeting, `STRING` interaction networks.

### 2. `pan-gene-family-analysis` — Pan-genome gene family analysis / 泛基因家族分析

Extends the single-genome skill from **one reference** to **many accessions / related species**, distinguishing **core (conserved)** members from **dispensable / private (variable, often linked to adaptation, domestication, stress resistance)** ones — and links that variation to phenotype.
把分析从「单个参考基因组」扩展到「多份种质 / 多个近缘物种」,区分**核心保守成员**与**可变/特有成员**(后者常与适应、驯化、抗逆相关),并把变异与表型挂钩。

**Covered steps / 覆盖步骤:**

- **Per-genome member identification / 跨种质成员鉴定** — reuses `gene-family-analysis` thresholds across all accessions with a *unified standard*.
- **Orthogroup clustering / 直系同源聚类** — `OrthoFinder` (or OrthoMCL / DIAMOND + MCL).
- **PAV matrix / 存在-缺失变异矩阵** — Presence/Absence Variation, the core data structure for all pan-level conclusions.
- **Core / dispensable classification & pan-core curve / 核心-非核心分类与增长曲线** — frequency binning + `Heaps' law`; open vs. closed pan-genome.
- **Copy number variation (CNV) / 拷贝数变异**.
- **Gene family expansion / contraction / 家族扩张与收缩** — `CAFE` along the phylogeny.
- **Structural variation (SV) detection / 结构变异检测** — `minimap2` + `SyRI` (assembly) or `Manta` / `Delly` / `Lumpy` (resequencing).
- **Selection & population genetics / 选择压力与群体遗传** — `Ka/Ks` (`PAML`), nucleotide diversity `π`, `Tajima's D`, `Fst`, selective sweeps.
- **Pan-transcriptome expression / 泛转录组表达谱**.
- **Variant–phenotype association / 变异-表型关联** — PAV / SV / CNV as genotype in `GWAS` (`GEMMA` / `GAPIT` / `EMMAX`); overlap with QTL / domestication intervals.
- **Functional interpretation / 功能解读** — core-conserved vs. accession-specific candidates for downstream validation and breeding.

---

## Manuscript proofreading & review prompts / 论文校对与审阅 prompts

A separate, **experience-based** prompt collection for the stage *after* the analysis is written up — turning a first draft into a submission-ready manuscript.
一组**基于作者亲身经验**的 prompt,服务于「分析写完之后」的环节 —— 把初稿打磨成可投稿的稿件。

Intended to help with:

- **Self-proofreading / 自我校对** — consistency of gene names, figure/table numbering, units, thresholds, and Methods reproducibility details.
- **Reviewer-style critique / 模拟审稿** — anticipating the questions a reviewer would actually ask about member identification rigor, phylogeny support, figure quality, and over-claiming.
- **Methods & reproducibility checks / 方法与可复现性核对** — verifying that genome versions, tool versions, parameters, and thresholds are all reported.

> This component captures the author's own reviewing experience rather than generic writing advice. 这部分沉淀的是作者真实的审稿经验,而非泛泛的写作建议。

---

## Design principles / 设计原则

1. **Reproducibility first / 可复现优先** — every step records tool versions, database versions, and key parameters, so a reviewer can redo it.
2. **Script-based final figures / 终稿图表优先脚本绘制** — publication figures are drawn with custom `Python` / `R` (`ggplot2` / `ggtree` / `pheatmap`) for quality and identity, avoiding the homogeneous defaults of GUI tools (`TBtools` / `GSDS` etc. are fallbacks for drafts or when no script environment is available).
3. **Species-specific databases over generalists / 优先物种专用数据库** — e.g. SGN for tomato, Cucurbit Genomics DB for cucurbits — prefer T2T / long-read assemblies, and always log the access date.
4. **Bilingual by design / 中英双语** — responds in the user's language while keeping gene / tool / database names and metrics in English.

---

## How to use / 如何使用

These are **Claude Skills**. Point an AI assistant that supports skills at the relevant `SKILL.md`, then describe your task in natural language — the skill guides the assistant through scoping questions, standard practice, and experience-based judgment calls.
本仓库是 **Claude Skills**。把支持技能的 AI 助手指向相应的 `SKILL.md`,再用自然语言描述任务即可 —— 技能会引导助手完成需求确认、标准做法和关键经验判断。

Example prompts / 示例提问:

- *"Identify all members of the WRKY family in tomato (Pfam PFxxxxx) and build the phylogeny."*
- *"我要在 20 份番茄种质里做某基因家族的泛基因组 PAV 分析,帮我规划流程。"*
- *"Review my gene-family manuscript draft the way a journal reviewer would."*

> Learn more about authoring skills: <https://docs.claude.com> · 关于技能的更多信息见 Anthropic 官方文档。

---

## Keywords / 关键检索词

`bioinformatics` · `gene family analysis` · `基因家族分析` · `pan-genome` · `泛基因组` · `pan gene family` · `泛基因家族` · `plant genomics` · `植物基因组学` · `crop genomics` · `作物基因组` · `phylogenetics` · `系统发育` · `OrthoFinder` · `PAV` · `presence absence variation` · `存在缺失变异` · `core dispensable genes` · `核心非核心基因` · `CAFE` · `gene family expansion contraction` · `synteny` · `共线性` · `MCScanX` · `Ka/Ks` · `HMMER` · `MEME motif` · `PlantCARE` · `cis-element` · `顺式元件` · `MAFFT` · `IQ-TREE` · `TBtools` · `expression heatmap` · `表达热图` · `structural variation` · `GWAS` · `selective sweep` · `Claude Skills` · `AI bioinformatics workflow` · `生信工作流` · `reproducible research` · `可复现分析` · `manuscript review` · `论文审阅` · `manuscript proofreading` · `论文校对`

### Suggested GitHub topics / 建议的 GitHub Topics

`bioinformatics` `gene-family` `pan-genome` `plant-genomics` `phylogenetics` `claude-skills` `ai-workflow` `reproducible-research` `crop-genomics` `comparative-genomics` `manuscript-review`

---

## Contributing / 参与贡献

Contributions that add new analysis skills, refine existing parameters with literature-backed defaults, or extend the review-prompt collection are welcome. Please keep the **reproducibility-first** and **literature-cited (no verbatim copying)** principles.
欢迎贡献新的分析技能、用有文献依据的默认值完善现有参数,或扩充审稿 prompt 集合。请保持**可复现优先**与**引用文献但绝不照搬原文**两条原则。

## License / 许可

本仓库内容采用 [CC BY 4.0](LICENSE) 授权。  
你可以自由使用、修改和分发，但需署名原作者。

## Disclaimer / 免责声明

These skills encode practical research experience and sensible defaults, but **defaults are starting points, not universal truths**. Always calibrate thresholds and tool choices to your specific species, data, and target journal.
本仓库沉淀的是实践经验与合理默认值,但**默认值只是起点,而非普适真理**。请始终结合你的具体物种、数据与目标期刊校准参数与工具选择。

This workflow serves as an **auxiliary tool** and does not replace the independent judgment of the researchers.
All AI-generated content must be verified by the researchers before it can be used for formal publication.
本工作流为**辅助工具**，不替代研究者的独立判断。  
所有AI生成内容须经研究者核实后方可用于正式发表。


---


## Citation / 引用 

If this workflow is helpful for your research, please cite it:
如果本工作流对你的研究有帮助，请引用：

> Yuhui Wang. (2026). *Gene Family Analysis AI-Assisted Workflow* (v1.1). GitHub. https://github.com/yuhuiwang1/gene-family-analysis-prompts

Alternatively, the citation format can be automatically generated using the `CITATION.cff` file located in the root directory of the repository.
或使用仓库根目录的 `CITATION.cff` 文件自动生成引用格式。


---

## Changelog / 版本记录 

| Version | Date| Updated|
|---------|-----|--------|
| v1.0 | 2026-06-01 | Initial release：Gene Family Manuscript Review SOP |
| v1.1 | 2026-06-24 | Updated SKILLs |


