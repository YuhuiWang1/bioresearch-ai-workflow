# 亚细胞定位预测工具:评价与命名对照 / Subcellular Localization Predictors

> 用途:在"亚细胞定位预测"步骤中,判断该信哪个工具、如何处理结果冲突时查阅本表。

## 工具评价与取舍 / Tool reliability & priority

| 工具 Tool | URL | 适用范围 / 优点 | 已知局限 | 你的信任度排序 |
|---|---|---|---|---|
| WoLF PSORT | https://wolfpsort.hgc.jp/ | 最为常用的一个预测网页，可选植物、动物和真菌，可预测11种亚细胞定位 | 在预测植物分泌蛋白时，灵敏度显著降低准确性降低 | [填:1/2/3...] |
| Plant-mPLoc | http://www.csbio.sjtu.edu.cn/bioinf/plant-multi/ | 主要针对植物蛋白的亚细胞定位，可预测12种亚细胞定位 | 预测结果中，一些蛋白对应多种亚细胞定位，为某些蛋白质的多个亚细胞定位提供了参考 | [填] |
| Cell-PLoc | http://www.csbio.sjtu.edu.cn/bioinf/Cell-PLoc-2/ | 这是针对不同物种蛋白的亚细胞定位网页集合入口| 根据物种选择其他定位入口 | [填] |
| DeepLoc | https://services.healthtech.dtu.dk/services/DeepLoc-2.1/ | 针对真核生物蛋白，有不同模型可供选择，推荐高质量模型 | [填] | [填] |
| LocTree3 | https://www.cs.cit.tum.de/bio/home/services/loctree3/ | 已失效，网页不见 | 已失效，网页不见 | 不推荐 |

> 以上工具评价持续更新

## 专一亚细胞定位工具 / Specific Tools

| 工具 Tool | URL | 预测亚细胞器 | 
|---|---|---|
| ChloroP | [URL] | 叶绿体 |
| Nucleo | URL | 细胞核 | 
| MitoProt | URL | 线粒体 | 
| PCLR | URL | 预测亚细胞器 | 
| PTS1 | URL | 预测亚细胞器 | 

> 以上工具列表持续更新

**冲突时的取舍规则:**

> 🧩 补充写出当 2 个以上工具结果不一致时,你按什么逻辑判断(例如:以哪个工具为主、
> 是否需要再用第三个工具仲裁、还是直接报告"定位存疑,需实验验证")。

**是否结合实验校正:**

> 🧩 补充写出你是否会用 GFP 融合蛋白亚细胞定位等实验结果反过来校正/否定预测结果,
> 以及在论文中如何措辞预测与实验结果不一致的情况。

## 命名对照表 / Terminology cross-reference

不同工具对同一细胞区室常用不同名称,容易被误读为不同结果。请在下表列出你确认过的
对应关系(占位行为示例格式,需替换为你核实过的真实对应):

| 区室(统一叫法) | WoLF PSORT 输出 | Plant-mPLoc 输出 | DeepLoc 输出 | 备注 |
|---|---|---|---|---|---|
| [统一叫法,如:叶绿体] | [该工具的原文标签] | [原文标签] | [原文标签] | [是否完全等价/有细分差异] |
| [统一叫法] | [ ] | [ ] | [ ] | [ ] | [ ] |

> 以上工具比对结果持续更新
