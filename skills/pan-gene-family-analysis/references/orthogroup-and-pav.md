# 直系同源聚类、PAV 与核心/非核心分类 / Orthogroups, PAV & core-dispensable

承接 SKILL.md 第 2–4 步。本文件展开 orthogroup 推断、PAV 矩阵生成、core/dispensable
分类与 pan/core 曲线的参数与判读。

## 目录 / Contents
- 1. OrthoFinder:运行与参数
- 2. 从 orthogroup 到 PAV / CNV 矩阵
- 3. core / softcore / dispensable / private 分类
- 4. pan/core 增长曲线与开放性判断
- 5. 常见坑

---

## 1. OrthoFinder:运行与参数

- **输入**:每份种质一个蛋白 fasta(只含最长转录本),文件名即物种/种质标识。
- **序列搜索引擎**:默认 **DIAMOND**(快);追求灵敏度可用 BLAST(慢)。
- **关键产出**:`Orthogroups.tsv`(orthogroup ↔ 各种质基因)、`Orthogroups.GeneCount.tsv`
  (计数矩阵,直接用于 CNV/CAFE)、单拷贝直系同源(可建种质/物种树)、可选的物种树。
- **家族层面**:全基因组跑完后,只保留**含目标家族成员的 orthogroups**(用参考家族成员
  ID 反查其所在 orthogroup)进入泛家族分析。
- 备选:OrthoMCL、DIAMOND + MCL(自定 inflation,默认 ~1.5;值越大簇越细)。

**版本与参数务必记录**(OrthoFinder 版本、搜索引擎、inflation),写进 Methods。

## 2. 从 orthogroup 到 PAV / CNV 矩阵

- **PAV(0/1)**:对每个家族 orthogroup × 每个种质,**该种质在该 orthogroup 有≥1 个成员 →
  1(present),否则 0(absent)**。
- **CNV(计数)**:同上但记**成员个数**(来自 `Orthogroups.GeneCount.tsv`),供拷贝数分析。
- 行 = 家族 orthogroup / 成员,列 = 种质;这张矩阵是后续一切泛层结论的底座。
- **质控**:警惕"假缺失"——某种质注释缺失或装配破碎导致的 0 并非真无该基因。对关键
  缺失,建议用该种质基因组做 tblastn 回查确认是真缺失还是注释漏掉。

## 3. core / softcore / dispensable / private 分类

按某成员**出现在多少比例种质里**分箱。**阈值需先与用户确认**;常见默认(可调):

| 类别 | 常见定义(可调) |
|---|---|
| **core** | 存在于 **100%** 种质 |
| **softcore**(near-core) | 存在于 **≥ ~90%**(或"除 1 份外") |
| **dispensable**(shell) | 存在于多于 1 份、但低于 softcore 阈值 |
| **private / unique**(cloud) | **仅 1 份**种质特有 |

注意事项:
- **样本数太少时分类不稳**:种质份数少(如 < ~10)时,softcore/dispensable 的比例阈值会
  非常敏感,应谨慎下结论或加大样本。
- softcore 的设立是为容忍**个别样本的装配/注释缺陷**造成的假缺失,阈值随样本量调整。
- 同一套阈值必须贯穿全文(SKILL.md 通用原则 4)。

> 🧩【你的经验 · 填这里】你惯用的具体百分比阈值、最少种质数要求、是否区分 shell/cloud。

## 4. pan/core 增长曲线与开放性判断

- **做法**:从 1 份种质开始,**逐步随机加入种质**,每一步记录 **pan-size**(到目前为止所有
  种质成员的并集)与 **core-size**(交集);因加入顺序影响结果,**每个样本数做多次随机
  抽样(如 100 次)取均值与分布**,画 rarefaction 曲线。
- **开放 vs 封闭**:pan 曲线随种质数增加**仍持续上升 → 开放(open)pan-genome**;**趋于
  饱和(平台) → 封闭(closed)**。常用 **Heaps' law**(\(n = \kappa \cdot N^{\gamma}\))拟合,
  由 γ 判断(γ 较大/不收敛 → 开放)。
- 工具:可用现成 pan-genome 流程(如基于基因 PAV 的 R/Python 脚本)或自写脚本拟合。

> 🧩【你的经验 · 填这里】你用的拟合模型、抽样次数,以及对"开放/封闭"的解读尺度。

## 5. 常见坑 / Pitfalls

- **注释口径不一致** → orthogroup 与 PAV 失真;跨种质务必统一注释/转录本处理。
- **旁系同源(paralog)与直系同源混入同一 orthogroup**:大家族尤甚,必要时结合系统发育
  拆分 orthogroup,避免把家族扩张误读成 PAV。
- **把"假缺失"当真缺失**:见第 2 节质控。
- **样本量小却强行分箱**:见第 3 节。
