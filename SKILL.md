---
name: xinyuan-cidian
description: Write or revise `心愿词典` psychology-science WeChat articles. Use when Codex needs to turn a life-adjacent topic, classic psychology theory, experiment, or effect into a public-science公众号稿 with the fixed section order `导言 -> Experiment -> Thinking -> Pleasure -> Summary -> Reference`, verified original studies, at least two figures/tables with captions, APA references, and strict no-fabrication constraints.
---

# Xinyuan Cidian

## Overview

生成或修订《心愿词典》心理学科普推文。文章必须一边保持公众号可读性，一边满足学术投稿要求：话题贴近生活，理论属于经典心理学范围，核心论证建立在可核验的原始实验与后续研究之上。

默认把文章写成“心理效应/理论名 -> 对应论文 -> 实验解析 -> 理论说明 -> 实践转化”的组合，而不是先凭印象写概念，再事后补文献。

## Inputs

从用户请求中提取这些输入：

- `topic_or_theory`：必填。优先是一个具体的经典心理效应、理论名或实验名。
- `working_title`：可选。若未给出，默认主标题沿用《心愿词典》并补一条主题副标题。
- `target_reader`：可选。默认面向普通公众号读者，并保持大学生也容易读懂。
- `existing_sources`：可选但高优先级。若用户给出 DOI、PDF、截图、论文题录或参考文献，优先使用并核验。
- `layout_sample`：可选但有约束力。若用户给出历史样稿、排版模板或配图范例，按其样式落稿。
- `output_format`：可选。默认先产出 Markdown；若用户明确要 DOCX、Word 可交稿版本，或给出 DOCX 样稿，则必须同时遵守 Word 版式规范。

## Workflow

1. 先读 [references/rubric.md](./references/rubric.md) 和 [references/output-template.md](./references/output-template.md)。
2. 如果用户给了历史 DOCX、要求 Word 成稿，或特别强调排版风格，再读 [references/word-layout-spec.md](./references/word-layout-spec.md)。
   - 把历史 DOCX 当成强约束，而不是“仅供参考”。
   - 优先复现最终视觉效果，不要迷信原文件里那些不稳定的样式名。
   - 若当前环境支持 `doc` skill，按其流程抽取和验证版式。
3. 先做“效应 -> 论文”映射：
   - 如果用户给的是具体心理效应、理论名或实验名，先找它最核心的原始论文、奠基研究或公认代表作。
   - 先确认该效应的英文标准名称、关键作者、发表年份、期刊或书籍来源。
   - 只有在找到并核验论文后，才允许进入写作。
   - 若当前环境提供 `$literature-search-extract`，优先用它完成关键词规划、元信息核验和实验细节提取。
4. 锁定题目：
   - 题目和导言必须围绕已经核验过的心理效应展开，而不是围绕模糊的人生感悟展开。
   - 如果用户给的是生活议题而不是效应，先把议题收束成一个经典心理效应，再去找对应论文。
   - 如果用户给的是实验名，先识别其背后的理论或效应，再决定文章标题和生活切口。
5. 建立证据包：
   - 先锁定 1 篇锚定论文：原始实验论文、奠基性论文，或该效应最常被引用的经典来源。
   - 再补 1 到 3 篇后续文献：复现、拓展、争议、改进范式、综述或元分析。
   - 提取并核验作者、年份、样本、关键变量、实验流程、主要结果、限制、DOI 或稳定链接。
   - 写作中所有关键结论都必须能追溯到这组已核验论文。
6. 规划图像与图表：
   - 优先寻找与已核验论文直接对应的图片材料。
   - 图片来源优先级：
     1. 开放获取论文正文中的 figure
     2. PMC / 出版社 figure 页面
     3. 根据论文原始数值重绘的图
   - 找图时同时记录：图号、图意、来源文章、来源链接、版权或开放许可信息。
   - 若原图不能稳定获取、版权不清晰，或分辨率太差，就不要硬贴原图，改为依据论文数据重绘。
7. 规划图表：
   - 图表 1：自变量、因变量、实验条件或范式结构表。
   - 图表 2：原论文中的数据图或数据表，优先直接来自原文。
   - 若原图是英文，基于原图翻译标签；若无法安全改图，可用经过核验的原始数值重建中文表格，并明确其来源。
8. 按固定结构写作：
   - `导言`：用具体场景、扎心问题或流行文化梗切入，先让读者觉得“这事和我有关”。
   - `Experiment`：解释实验问题、设计、样本、流程、控制混淆变量的方法、主要结果，以及后续拓展或争议。
   - `Thinking`：完整解释该理论的概念、机制、边界条件和适用范围。
   - `Pleasure`：把理论迁移到关系、学习、自我成长、工作或传播场景，并给出可执行建议。
   - `Summary`：收束全文，避免拔高和空泛煽情。
   - `Reference`：只列出真实核验过的参考文献，并使用 APA 风格。
- 在成稿中，至少安排 1 到 2 张与论文直接相关的图片或重绘图，默认优先放在 `Experiment` 部分。
9. 若需要交付 Word 成稿：
   - 先把 Markdown 结构映射到 Word 段落。
   - 标题、导入钩子、正文章节标题、正文、表格标题、参考文献、署名分别按 [references/word-layout-spec.md](./references/word-layout-spec.md) 设定。
   - 图像优先使用嵌入式 inline 图片，不使用悬浮绕排。
   - 图片下方补图注，写清“图里展示了什么”和“来源于哪篇论文”。
10. 收尾前，对照 [references/source-checklist.md](./references/source-checklist.md) 做预检。

## Hard Rules

- 不要抄写其他公众号、自媒体稿件或低质量 AI 成稿。
- 不要编造实验、被试量、数值结果、统计结论、图表、引文、作者、年份、DOI 或参考文献。
- 在存在国外原始文献时，不要只依赖国内转述或自媒体二手材料。
- 如果用户给了一个具体心理效应，必须先找到对应论文，再开始写；不允许先写正文、后补文献。
- 如果关键事实无法核验，明确标记缺口或暂停，不要用“常识”补空。
- 如果找不到可信论文，就停在“待核验”状态，不要硬写成稿。
- 不要把文章写成实验流程流水账；`Thinking` 和 `Pleasure` 必须承担解释与转化功能。
- 不要跳过 `Reference`。
- 不要漏掉至少两张图表及其图注/表注。
- 如果交付 Word 或图文成稿，默认需要补至少 1 张论文相关图片；优先开放获取 figure，次选重绘图。
- 一级标题必须按 `导言 -> Experiment -> Thinking -> Pleasure -> Summary -> Reference` 顺序出现。
- 二级标题统一使用 `»`。
- 核心概念与重要结论要加粗，图表与图注默认居中呈现。
- 若交付 Word 成稿，优先匹配样稿的版式观感：A4 页面、居中总标题、左对齐英文分节标题、两字符首行缩进正文、右对齐署名。

## Output Notes

- 默认输出 Markdown 草稿，便于后续复制到公众号编辑器或 Word 模板中。
- 若用户未提供总标题，默认标题格式使用 `心愿词典第{期数}期丨{主题}`；若期数未知，可暂写 `心愿词典丨{主题}`。
- 若文字段落过密，主动在适当位置安排配图建议，但不要用无意义占位词。
- 图注和表注要讲“这张图说明了什么”，而不只是重述图名。
- APA 参考文献里优先保留 DOI；如果没有 DOI，至少给出稳定来源信息。
- 当前默认 Word 观感以 `Vol.8` 样稿为基线：标题 16pt 微软雅黑加粗，章节标签 14pt 微软雅黑加粗，正文以微软雅黑为主并保留两字符首行缩进。

## Resources

- [references/rubric.md](./references/rubric.md)：评分标准、硬性门槛和一票否决项。
- [references/output-template.md](./references/output-template.md)：固定 Markdown 骨架和各部分写作提醒。
- [references/source-checklist.md](./references/source-checklist.md)：选题、文献、图表、APA 的交稿前核对表。
- [references/word-layout-spec.md](./references/word-layout-spec.md)：基于 `心愿词典Vol.8.docx` 提炼出的 Word 标题、字体、段落、图表和署名版式规范。
