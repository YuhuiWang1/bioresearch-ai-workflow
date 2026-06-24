# 保守基序 (Motif) 分析:参数、解读与作图 / Conserved Motif Analysis

> 出处 / Source:作者科普文章《Motif分析,从生信到生物学意义》(微信公众号):
> https://mp.weixin.qq.com/s/0S7HJYDJkTcUyABTAnLHPw
>
> **范围说明**:原文是一篇较完整的 Motif 学习笔记,还涵盖 ChIP-seq/ATAC-seq 的
> TF 结合位点分析、JASPAR/TRANSFAC 等转录因子数据库、Wet-lab 验证(EMSA/ChIP/Y1H/
> 双荧光素酶)等内容。**本文件只提取与"基因家族保守基序分析"直接相关的部分**
> (对家族成员蛋白序列做 de novo motif 发现)。原文其余内容在做启动子顺式元件
> (见 SKILL.md 第 11 步)或专门的 TF 结合位点研究时再另行参考。

## 0. 本步定位 / Scope
基因家族里的 motif 步骤 = 对**家族成员的蛋白序列**做 de novo 保守基序发现,用于辅助
亚家族划分与功能推断,通常与基因结构、系统发育树拼成一张组合图。

## 1. Motif 的展示形式(读结果前先认识) / Representations
- **Logo**:X 轴为 motif 序列位置,Y 轴为信息量(bits),字母相对大小表示该位置
  碱基/氨基酸的出现频率。
- **一致性序列 Consensus**:用一条简并序列表示 motif,简并碱基遵循 IUPAC 命名法。
- **PFM(位置频数矩阵)/ PWM(位置权重矩阵)**:PFM 记录每个位置各碱基的频数;
  PWM 由 PFM→PPM(频率)再用背景碱基频率校正而来,可对一段序列打分(≥0 为潜在功能
  位点,<0 视为随机序列)。不同软件展示略有差异,核心内容一致。

## 2. 工具选择(以 MEME Suite 为主) / Tools
- **MEME(de novo 发现)**:基于输入序列独立计算出全新 motif(不与数据库比对、也不是
  用已知模型扫描),输出 motif 结构 + 其在序列上的位置;**不允许 motif 含空位
  (ungapped)**。
- **关键点(最易被忽略)**:MEME 的挖掘阈值是 **"motif 的数量",而不是可信度
  (E-value)**。因此必须:(a) 指定想从序列中挖掘的 motif 数量;(b) **再逐个检查
  所挖 motif 的 E-value**,剔除不可信的。
- **MEME vs STREME**:同一输入下,**MEME 挖到的 motif 偏长(不少长度 >20),STREME
  偏短(约 10)**;当序列**数量较多(>50)**时,STREME 更适合挖掘全新、富集的短 motif。
- **GLAM2**:与 MEME 类似,但**允许 motif 含空位**;需要带 gap 的 motif 时使用。
- 注:CentriMo / AME / SpaMo(富集)、FIMO / MAST / MCAST(扫描)等多用于 TF 结合
  位点/测序数据场景,基因家族保守基序分析一般用不到(Tomtom 例外,见下)。

### 2.1 MEME 关键参数设定 / Key MEME parameters

**(1) Motif 数量(`-nmotifs`)** — 工具在序列中寻找的 motif 最大数量。
- 常规:一般设 **5 / 10 / 15 / 20**;序列较长、数据量较大或需较多保守模式时可设 **20 或 30**。
- 短序列:降低数量(如 **3 或 5**),避免过度拆分导致结果不可靠。
- 迭代:若初次分析后部分基因无 motif 结果,可增大数量(如 10 → 20)以扩大搜索范围。

**(2) Motif 宽度范围(`-minw` / `-maxw`)** — 限定 motif 的最小与最大长度。
- 常规:`-minw` 一般设 **6**;`-maxw` 设 **50**(MEME 默认)或 **200**(较长序列或蛋白序列)。
- 按数据类型适配:**DNA 序列**(如 ATAC-seq 的 peak)motif 通常较短,常见 **6–20 bp**;
  **蛋白序列**(氨基酸)motif 相对较长,常见 **6–200 aa**。→ 基因家族用的是蛋白序列,
  故 `-maxw` 常需放宽到 200。
- 经验调整:若未找到理想 motif,可扩大 `-maxw`(如调至 200)或放宽范围,以适配未知 motif
  的真实长度。

## 3. 把 motif 与已知 motif / 结构域对应 / Annotation
- **Tomtom**:将 de novo 挖到的 motif 与已知 motif 数据库比对,找出相似 motif,用于
  **注释 motif 的潜在身份**。这是把"统计意义上的 motif"连接到"生物学意义"的关键一步;
  也可进一步与 Pfam / InterPro 家族核心结构域交叉印证。

## 4. 可视化(遵循 SKILL.md 通用原则:终稿用脚本自定义) / Visualization
- 需按全文配色/尺寸重绘 motif Logo 时,优先用脚本而非工具默认图:**WebLogo**、
  **R/seqLogo**、**R/ggseqlogo**。
- **R/motifStack**:把多个 motif 聚合(stacked / circle),按 motif 间距离构建进化树,
  分析 motif 在物种间/基因间的进化关系。
- 通常把 motif 图与**基因结构、系统发育树**拼成一张组合图。

## 5. 联合分析(可选) / Joint analysis
- 若有 RNA-seq:可将 motif/调控预测与基因表达变化关联(共表达提示潜在靶基因),做更具
  生物学意义的解读。(原文还涉及 ChIP-seq/ATAC-seq 联合,属 TF 研究范畴,按需取用。)

## 6. 常见误区 / Pitfalls
- 只看 motif 数量、**不检查 E-value**,把不可信 motif 当结论(再次强调:MEME 阈值是
  数量、不是可信度)。
- 序列数很多(>50)时仍硬用 MEME 而非 STREME,结果不佳。
- 把 de novo motif 直接当成"有功能的结构域"过度解读——需经 Tomtom 注释或与
  Pfam/InterPro 结构域交叉印证后再下结论。

> 将持续更新：哪些 motif 实为核心结构域、motif命名/标注习惯。
