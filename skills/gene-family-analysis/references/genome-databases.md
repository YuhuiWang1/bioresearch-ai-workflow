# 物种专用基因组数据库清单 / Species-specific Genome Databases

> 用途:在"家族成员鉴定"前选择基因组来源时查阅本表。**优先使用研究物种自己的专用数据库**;
> Phytozome / NCBI 的版本常落后,仅用于交叉核对。多版本时取**T2T > 长读长(PacBio 等)> 最新发布版**。
>
> 出处 / Source:本清单整理自作者的科普文章(微信公众号,收录 20+ 物种专用基因组数据库):
> https://mp.weixin.qq.com/s/xKmBXymtp59zjEy6cfIxBg
> 注:AI 工具通常无法直接打开微信公众号链接,故清单实际内容以本表为准;链接仅作扩展阅读与署名出处。

## 如何使用本表

1. 在表中查找目标物种 / 类群,使用对应的 **首选数据库 URL**。
2. 进入后选择组装版本:优先 **T2T**,其次 **长读长组装**,再次 **最新发布版**。
3. 记录:数据库名、组装版本号、注释版本号、访问日期。

## 数据库对照表 / Lookup table(作者整理,源自上述文章)

| 物种 / 类群 Species or clade | 首选数据库 Primary database | URL | 备注 Notes |
|---|---|---|---|
| 番茄 Tomato (*Solanum lycopersicum*) | SGN (Sol Genomics Network) | https://solgenomics.net/ | 茄科多物种、注释更新及时:番茄、马铃薯、辣椒、烟草、茄、枸杞、曼陀罗等 |
| 葫芦科作物 Cucurbits | Cucurbit Genomics Database | http://cucurbitgenomics.org/v2/ | v2 含多个高质量组装:黄瓜、甜瓜、西瓜、南瓜/笋瓜等 |
| 莴苣(生菜)Lettuce | Lettuce Genome Resource | http://lgr.genomecenter.ucdavis.edu/Home.php | 莴苣 *Lactuca sativa* 及近缘种基因组与注释(UC Davis) |
| 蔷薇科 Rosaceae | Genome Database for Rosaceae (GDR) | https://www.rosaceae.org | 苹果、梨、桃、杏、李、樱桃、草莓、玫瑰、树莓等 |
| 大豆 Soybean | SoyBase | https://www.soybase.org/ | 大豆 *Glycine max* 基因组、遗传图谱、QTL、表达数据(USDA) |
| 豆类 Pulse crops | Pulse Crop Database (PCD) | https://www.pulsedb.org/ | 鹰嘴豆、小扁豆、豌豆、普通菜豆等食用豆类 |
| 柑橘属 Citrus | Citrus Genome Database | https://www.citrusgenomedb.org/ | 柑橘属多物种基因组、遗传与育种数据 |
| 甜橙 Sweet orange | Orange (*Citrus sinensis*) Genome Annotation Project | http://citrus.hzau.edu.cn/orange/ | 甜橙专用注释(华中农业大学) |
| 越橘属 Vaccinium | Genome Database for Vaccinium (GDV) | https://www.vaccinium.org/ | 蓝莓、蔓越莓等越橘属作物 |
| 芸薹属/十字花科 Brassica | **BRAD V3.0**(Brassicaceae Database) | **http://brassicadb.cn** | **已由 V2.0 升级至 V3.0,收录 26 个十字花科物种**(白菜、甘蓝、油菜、芥菜、萝卜等);旧址 brassicadb.org/brad 为 V2.0 |
| 杨属(杨/白杨)Populus | AspenDB | http://aspendb.uga.edu/ | 717 杂种杨(*P. tremula × alba*)等 Populus 基因组,常用于转基因;更广杨属资源见 PopGenIE / PlantGenIE (https://plantgenie.org) |
| 水稻 Rice | Rice Genome Annotation Project (RGAP) / RAP-DB | http://rice.uga.edu/ ; https://rapdb.dna.affrc.go.jp/ | 水稻 *Oryza sativa* 两大权威注释库 |
| 玉米 Maize | MaizeGDB | https://www.maizegdb.org/ | 玉米 *Zea mays* 社区数据库(USDA) |
| 棉花 Cotton | CottonGen | https://www.cottongen.org/ | 棉属多物种基因组、遗传、育种 |
| 拟南芥 Arabidopsis | TAIR (The Arabidopsis Information Resource) | https://www.arabidopsis.org/ | 拟南芥 *A. thaliana* 权威库;**关键参照物种**(注释 Araport11) |
| 猕猴桃 Kiwifruit | Kiwifruit Genome Database (KGD) | http://kiwifruitgenome.org/ | 猕猴桃属(*Actinidia*)多组装、注释、转录组;另有泛基因组库 KPGD |
| 葡萄 Grape | Grapedia(Grape Genomics Encyclopedia) | https://grapedia.org/ | 含 PN40024 的 **T2T** 参考基因组;另 Grapegenomics.org、URGI 可选 |
| 番木瓜 Papaya | NCBI Assembly | https://www.ncbi.nlm.nih.gov/datasets/genome/?taxon=3649 | **物种共 32 条组装记录(taxon=3649)**,从中挑注释完整、版本最优者。可用例:**`GCA_051312835.1`(ASM5131283v1,品种 AU9_5,福建农林)**,FTP: ftp.ncbi.nlm.nih.gov/genomes/all/GCA/051/312/835/GCA_051312835.1_ASM5131283v1/(下前确认目录含 `_genomic.gff.gz`/`_protein.faa.gz`)。带注释退路:`GCF_000150535.2`(SunUp/Papaya1.0,RefSeq)。**注意:Zihui(广东农科院,PRJNA968045)虽有组装,但论文宣称的 Figshare/补充注释实际未提供 CDS/GFF3,不可直接用**。 物种有多套种质组装 → 适合**泛基因家族分析**(见姊妹技能 pan-gene-family-analysis) |
| 小麦 Wheat | Ensembl Plants(IWGSC RefSeq) | https://plants.ensembl.org/Triticum_aestivum/ | 小麦基因家族常用标准参考;**异源六倍体 AABBDD**,注意同源三联体(homeolog triad);命名参考 GrainGenes,整合门户另有 WheatOmics |
| 多物种综合 Multi-species | Gramene | https://www.gramene.org/ | 跨物种比较基因组学门户(含多种作物) |
| 物种 / 类群 | 数据库 | URL | 备注 |

>以上数据库持续更新中

## 通用兜底来源 / Fallback (当某物种没有专用库时)

按需选用,并在 Methods 标明版本:
- Ensembl Plants — https://plants.ensembl.org/
- Phytozome (JGI) — https://phytozome-next.jgi.doe.gov/ (注意版本可能滞后)
- NCBI Genome / Datasets — https://www.ncbi.nlm.nih.gov/datasets/ (注意版本可能滞后)
