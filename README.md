# bioresearch-ai-workflow

AI-assisted workflow for bioinformatic research, e.g. gene family analysis, RNA-seq, Methy-seq, DNA-seq. 

# Gene Family Analysis — AI-Assisted Workflow
**基因家族分析 AI 辅助工作流**

> Structured prompts (SOPs) for gene family identification, evolutionary analysis, and manuscript writing — for plant scientists and related fields.
> 本仓库收录用于基因家族鉴定、进化分析与论文写作的结构化提示词（SOP），面向植物科学及相关领域科研人员。  


---

##How to use  / 如何使用

1. Enter the `workflow/` directory and select the corresponding `.md` file based on the analysis stage. 进入 `workflow/` 目录，按分析阶段选择对应的 `.md` 文件
2. Copy and paste the prompt text completely into the large language models such as Claude, ChatGPT, or DeepSeek. 将提示词完整复制粘贴到 Claude / ChatGPT / DeepSeek 等大语言模型
3. Replace the placeholders within the `[square brackets]` with your own species and data. 根据自己的物种和数据替换 `[方括号]` 内的占位符
4. If you encounter any issues or have suggestions for improvement, please raise an [Issue](../.. /issues) 如遇到问题或有改进建议，请提 [Issue](../../issues)

---

##Structure /目录结构 

```
.
├── README.md
├── LICENSE (CC BY 4.0)
├── CITATION.cff
└── workflow/
    ├── 01_Gene Family Manuscript Review SOP   #
        ├── 011_gene_identification.md         # 基因家族鉴定
        ├── 012_evolutionary_analysis.md       # 进化分析
        ├── 013_functional_annotation.md       # 功能注释
        ├── 014_manuscript_writing.md          # 论文写作
        └── 015_manuscript_review.md           # 论文核查（完整SOP）
```

---

## Citation / 引用 

If this workflow is helpful for your research, please cite it:
如果本工作流对你的研究有帮助，请引用：

> Yuhui Wang. (2026). *Gene Family Analysis AI-Assisted Workflow* (v1.0). GitHub. https://github.com/yuhuiwang1/gene-family-analysis-prompts

Alternatively, the citation format can be automatically generated using the `CITATION.cff` file located in the root directory of the repository.
或使用仓库根目录的 `CITATION.cff` 文件自动生成引用格式。

---

## Contributing /贡献 

Welcome to participate in the improvement through the following methods:
欢迎通过以下方式参与改进：

- **Issue**
- **PR**
- **Discussion**：[Discussions](../../discussions) shared your Experience


---

## Changelog / 版本记录 

| Version | Date| Updated|
|---------|-----|--------|
| v1.0 | 2026-06-01 | Initial release：Gene Family Manuscript Review SOP |

---

## License / 许可证

本仓库内容采用 [CC BY 4.0](LICENSE) 授权。  
你可以自由使用、修改和分发，但需署名原作者。

---

## Disclaimer / 使用声明

This workflow serves as an **auxiliary tool** and does not replace the independent judgment of the researchers.
All AI-generated content must be verified by the researchers before it can be used for formal publication.
本工作流为**辅助工具**，不替代研究者的独立判断。  
所有AI生成内容须经研究者核实后方可用于正式发表。
