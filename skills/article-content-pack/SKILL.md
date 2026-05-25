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
4. Keep the voice professional, direct, and human. Avoid generic AI writing, inflated adjectives, repetitive transitions, and overly tidy "first/second/third" structures unless the content naturally needs them.
5. Write `topics.md` with platform-ready related topics/hashtags and short rationale.
6. Write `publish.md` with at least 5 title options in different styles, at least 5 description copy options of no more than 30 Chinese characters each, caption, hashtags, pinned comment, CTA reply seed, recommended publishing time windows for the target platform, and manual publishing notes.
7. Write `cover.txt` as the cover-generation brief. The cover count and content must align one-to-one with the recommended title options in `publish.md`; create one cover prompt for each title option, with a minimum of 5 cover prompts.
8. Generate one finished cover image for each recommended title option under `covers/` when image generation is available, with a minimum of 5 images. Use `gpt-image2` by default for image generation. Name them `cover-01`, `cover-02`, `cover-03`, etc. in the same order as the `publish.md` title options, with the native image extension returned by the tool, such as `.jpg`, `.jpeg`, or `.png`.
9. Write `review.md` with factual, copyright, privacy, AI-labeling, cover consistency, and human sign-off checks.
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

If image generation is unavailable, `gpt-image2` is unavailable, or the user asks for text-only output, still write `cover.txt` and clearly mark missing cover images or model limitations in `review.md`.

## Content Rules

- Do not invent facts, cases, customer quotes, numbers, benchmarks, endorsements, or screenshots.
- Do not promise guaranteed revenue, traffic, follower growth, ranking, or platform approval.
- If the topic depends on current software behavior, documentation, prices, laws, policies, or product capabilities, verify with current official sources before writing.
- For business, education, health, finance, legal, recruitment, minors, or current-event topics, flag uncertain claims for human fact-checking.
- Keep cover titles, description copy, and publishing copy consistent with `article.md`; do not add stronger claims in the packaging than the article supports.
