# 诗史画册插画

新增众家卷首页的六张独立背景，见 [六境背景与完整提示词](frontispiece/README.md)。

## river-ink.webp

- 用途：卷首封面、黄州章节画页、作品装帧与页边山水。
- 创建日期：2026-11-12。
- 创作方式：Codex 内置 imagegen 生成，由项目转码为 WebP（1536 × 1024，质量 88），减少页面传输体积。
- 内容性质：AI 辅助原创艺术插画，非馆藏复制、历史地点复原或史实证据。页面的作品原文和资料来源仍来自原有内容库。
- 使用：随 PoetEra 项目使用，遵循仓库 LICENSE；不暗示历史真实性或专有版权。
- 本文件记录最终使用的完整提示词。原 SVG 人物与山水资源的署名、许可及使用说明保留在 `content/phase-01.json`。

最终提示词：

> Use case: illustration-story. Asset type: a wide landscape painting for the cover and chapter plates of a refined Chinese poetry reading website, PoetEra. Generate an original museum-quality Chinese literati ink wash landscape painting on warm ivory rice paper, horizontal aspect ratio 3:2 or wider. An immense quiet river with a very tiny solitary wooden fishing boat slightly right of center near the lower third, mist drifting between layered expressive dark charcoal and muted moss-gray mountains, steep intricately textured crags and sparse pine trees along the right bank, distant faint mountains at left. Strong, organic dry-brush details, soft watery ink blooms, tactile handmade paper grain. Visually arresting but contemplative, inspired by the spacious composition of Song dynasty landscape painting, sophisticated contemporary art book treatment. The upper left 40 percent should be mostly empty warm paper and subtle pale mist, the right half richly detailed dark mountain landscape; the lower left has a small loose ink shoreline. Colors: warm ivory #eee9de, charcoal #2e352c, subdued moss #707665, faint umber. Broad soft white mist bands, no blue color. Flat scanned painting, not a photograph of a book, no frame. Absolutely no text, letters, calligraphy, seals, logos, UI elements or watermark. Artistic landscape, not a factual reconstruction of any historic location.

## su-shi-poetry-vertical.webp

- 用途：苏轼生平默认“行旅长卷”背景。
- 创建日期：2026-11-13。
- 创作方式：Codex 内置 imagegen，按用户修订改为纯文字、全部竖向排版；八组摘句以字号、起笔高低与墨色形成错落。
- 文件：1774 × 887，WebP 质量 92，约 112 KiB；不依赖外部图片服务。
- 不含山水、舟船或肖像。它是 AI 辅助的诗句排版艺术，不是苏轼手迹。
- 诗句出处列于 `content/su-shi-journey.json`，在页面“诗句出处 · 图记”中可查。采用简体，标点按画面调整。
- 最终编辑提示词：[su-shi-poetry-prompt.txt](su-shi-poetry-prompt.txt)。编辑输入为同次生成的纯诗句背景，最终替换为全竖排版本。
- 使用：随 PoetEra 项目使用，遵循仓库 LICENSE；不暗示历史真实性或专有版权。
