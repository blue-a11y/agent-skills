---
name: article-content-pack
description: Generate publish-ready Chinese article content packs with article body, summary, cover brief, cover images aligned to publish titles, related topics, publishing copy, and review checklist. Use when the user asks for 文章创作, 图文文章, 公众号文章, 小红书/抖音图文文案, 技术文章, 封面图, 话题标签, 发布文案, or a packaged article deliverable.
---

# Article Content Pack

Create one publish-ready article package. This skill is article-first; do not create video scripts, shot lists, subtitles, HyperFrames projects, or video publishing assets.

Before generating or reviewing a package, read `references/article-pack-spec.md`. Use `assets/templates/` only as skeletons; final files must contain task-specific content, not placeholders.

## Before Generation

Before creating files, collect or confirm the user's inputs with the natural-language template in `assets/templates/request.md`.

If the user has already provided enough information, briefly summarize the resolved inputs and proceed. If key fields are missing, show the compact natural-language template in a copyable code block with recommended defaults filled in, and ask the user to edit or confirm it. Do not generate the package until the user provides or confirms the input template.

## Output Location

Never write generated article packages inside this skill directory.

Use this location policy:

1. If the user provides an output directory for the current request, use it and save it as the preferred base directory.
2. Otherwise, read the preferred base directory from `~/.codex/article-content-pack/config.json`.
3. If no saved preference exists, ask the user where to store article packages. Offer `~/Desktop/artcle-create` as the default. After the user answers, save the chosen base directory to `~/.codex/article-content-pack/config.json`.
4. If the user asks you to proceed without answering, use `~/Desktop/artcle-create` and save it as the preferred base directory.

Within the base directory, create one package folder named `YYYY-MM-DD-topic-slug/`.

## Workflow

1. Collect or confirm the natural-language input template from `assets/templates/request.md`.
2. Normalize the request into `brief.yaml`: topic, audience, account positioning, platform, style, CTA, assumptions, and risks.
3. Write `article.md` with summary and body only. Do not put an article title in `article.md`; all title options belong in `publish.md`. Keep the summary within 30 Chinese characters by default unless the user asks for a longer one. Use natural paragraph breaks and a small number of `##` headings when they help scanning.
4. Make the article feel like an account work, not a neutral explainer. Include a clear author judgment, one concrete usage scene or decision moment, and one practical boundary such as "I would use it for..." or "I would not use it for...". Avoid generic AI writing, inflated adjectives, repetitive transitions, and overly tidy "first/second/third" or "适合三类人" structures unless the content naturally needs them. Avoid repeating the "不是...而是..." pattern more than once. After drafting, run a voice pass: replace generic transitions, template contrasts, and broad claims with concrete words from the article's object, workflow, artifact, or tradeoff. A package must not be approved if the body is mainly abstract method labels. It needs at least one copyable concrete artifact, such as a role card, prompt block, handoff format, command/checklist, before/after example, or decision table that could only belong to this topic.
5. Write `topics.md` with platform-ready related topics/hashtags and short rationale.
6. Write `publish.md` with exactly 5 title options in different styles, exactly 5 description copy options of no more than 30 Chinese characters each, caption, hashtags, pinned comment, CTA reply seed, recommended publishing time windows for the target platform, and manual publishing notes. Description copy is for publishing cards, feeds, and cover/list previews; it must not duplicate the `article.md` summary verbatim. The caption must be rewritten for platform reading, not just compressed from `article.md`; it should open with a concrete tension, observation, or question.
7. Write `cover.txt` as the cover-generation brief. The cover count and content must align one-to-one with the 5 recommended title options in `publish.md`; create exactly 5 cover prompts. Cover visuals should fit the topic and audience, avoid excessive sci-fi or generic AI aesthetics, and avoid over-templated layouts that erase the specific character of the work. Prefer minimalist editorial covers: clear title area, one topic-specific visual anchor, restrained colors, and enough negative space. Minimalist does not mean generic: each cover needs at least one visual or text detail that belongs to the current topic and would not fit any random productivity article. Do not use a plain three-item checklist or generic knowledge-card layout unless it is anchored by concrete words, artifacts, or tradeoffs from the article. Do not put visible sequence numbers, corner badges like `01/02/03`, batch labels, template labels, or repeated footer branding into the cover image unless the user explicitly asks; numbering belongs only in filenames and `publish.md` ordering. Do not default to desk flat-lays, keyboards, neon, holograms, futuristic HUDs, floating panels, generic AI brains, robot heads, circuit-board decoration, or fake product UI. Use a keyboard only when the user asks for it or when it is genuinely necessary for the scene; if included, make it a white Apple Magic Keyboard-style keyboard by default unless the user asks for another style.
8. Generate exactly 5 finished cover images under `covers/` when image generation is available. Use the Codex生图工具 by default for background or visual-anchor generation, but do not rely on the image model to render Chinese title text. Render the final title and any required short labels with local deterministic typography or post-processing when possible, so the text exactly matches `publish.md`. Use a thick, stable, cover-grade Chinese title font; avoid thin, tall, narrow, or default-looking system fonts. On macOS, prefer `/System/Library/Fonts/Hiragino Sans GB.ttc` index 2 (`Hiragino Sans GB W6`) for simplified Chinese cover titles; do not use Japanese-only Hiragino W8/W9 as the default because some simplified Chinese glyphs can be missing. If the available font is not heavy enough, add subtle stroke, shadow, or stronger size/spacing rather than switching to a thin font. Name final files `cover-01`, `cover-02`, `cover-03`, `cover-04`, and `cover-05` in the same order as the `publish.md` title options, with the native image extension returned by the tool, such as `.jpg`, `.jpeg`, or `.png`.
9. Write `review.md` with factual, copyright, privacy, AI-labeling, cover consistency, account voice, and human sign-off checks. Include a short voice diagnosis in the notes: what makes this package recognizable for the account, what still feels template-like, and what was rewritten to reduce AI writing smell. Include a text-smell audit with phrase counts for generic transitions and banned generic claims. If unresolved template risk remains, mark the decision as Revise, not Approve.
10. Run the completion check from `references/article-pack-spec.md` before finishing.

## Output Contract

```text
brief.yaml
article.md
topics.md
cover.txt
covers/cover-01.(jpg|jpeg|png)
covers/cover-02.(jpg|jpeg|png)
covers/cover-03.(jpg|jpeg|png)
covers/cover-04.(jpg|jpeg|png)
covers/cover-05.(jpg|jpeg|png)
publish.md
review.md
```

Use lowercase ASCII slugs when possible.

If image generation is unavailable or the user asks for text-only output, still write `cover.txt` and clearly mark missing cover images or tool limitations in `review.md`.

## Content Rules

- Do not invent facts, cases, customer quotes, numbers, benchmarks, endorsements, or screenshots.
- Do not promise guaranteed revenue, traffic, follower growth, ranking, or platform approval.
- If the topic depends on current software behavior, documentation, prices, laws, policies, or product capabilities, verify with current official sources before writing.
- When citing official sources, follow redirects and use the current canonical URL in `article.md` rather than an old redirected URL.
- For business, education, health, finance, legal, recruitment, minors, or current-event topics, flag uncertain claims for human fact-checking.
- Keep cover titles, description copy, and publishing copy consistent with `article.md`; do not add stronger claims in the packaging than the article supports.
- The final package should read like a recognizable account post: concrete, opinionated, useful, and restrained. If a sentence could fit almost any AI tool article, rewrite it with the current topic's actual object, scene, or tradeoff.
