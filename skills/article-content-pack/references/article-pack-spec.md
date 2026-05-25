# Article Pack Specification

Use this reference when generating or reviewing an `article-content-pack` output directory.

## Required Directory

Packages are written under the configured base directory, not inside the skill directory. Default base directory:

```text
~/Desktop/artcle-create
```

Package folder:

```text
<base-directory>/YYYY-MM-DD-topic-slug/
  brief.yaml
  article.md
  topics.md
  cover.txt
  covers/
    cover-01.(jpg|jpeg|png)
    cover-02.(jpg|jpeg|png)
    cover-03.(jpg|jpeg|png)
    cover-04.(jpg|jpeg|png)
    cover-05.(jpg|jpeg|png)
  publish.md
  review.md
```

Final files must be filled with real content. Do not leave blank headings, empty table cells, placeholder bullets, `TODO`, angle-bracket placeholders, or standalone placeholder hashtags.

## Output Location Memory

Preferred base directory is stored in:

```text
~/.codex/article-content-pack/config.json
```

Expected shape:

```json
{
  "base_directory": "/Users/<user>/Desktop/artcle-create"
}
```

When no preference exists and the user has not provided an output directory, ask once where to store article packages. Offer `~/Desktop/artcle-create` as the default. Save the answer before writing package files.

## Input Template

Before writing files, collect or confirm the input fields in `assets/templates/request.md`. The request template is compact natural language rather than YAML, and should be shown in a copyable code block with recommended defaults when fields are missing.

Required minimum fields:

- topic
- target audience
- account positioning
- platform
- style
- content goal or CTA

Optional fields improve quality:

- article angle
- must-mention points
- avoid points
- references
- cover style
- output base directory

## brief.yaml

Required fields:

```yaml
topic:
target_audience:
account_positioning:
platform:
style:
content_goal:
cta:
output_directory:
created_at:
core_message:
assumptions:
  - 
risk_notes:
  - 
```

## article.md

Required sections:

```markdown
## 摘要

## 正文

## <2-4 natural section headings when useful>

## 参考资料
```

Do not include a `#` article title or any title options in `article.md`; all title options belong in `publish.md`. Use 800-1500 Chinese characters by default unless the user requests another length. Keep `## 摘要` within 30 Chinese characters by default unless the user explicitly asks for a longer summary. Prefer concrete observations and operational advice. Use headings to improve scanning, not to create a rigid outline.

## topics.md

Include:

- recommended hashtags/topics
- rationale for topic groups
- topics to avoid when relevant

## cover.txt

Include:

- the same title options from `publish.md`, in the same order
- 2-3 visual directions
- design notes for safe area, contrast, and text length
- default device styling: when a keyboard appears, use a white Apple Magic Keyboard-style keyboard unless the user requests another keyboard style
- one cover prompt for each title option in `publish.md`, with a minimum of 5 cover prompts
- the intended output filename for each prompt, such as `covers/cover-01.png`
- risky claims or words to avoid

## covers/

Generate one finished cover image for each recommended title option in `publish.md` when image generation is available, with a minimum of 5 images. Use `gpt-image2` by default. If the available image tool cannot explicitly select a model, record that limitation in `review.md` and still generate the images with the best available image tool. Use the native output format from the image tool; JPG, JPEG, and PNG are all acceptable. Use:

```text
covers/cover-01.(jpg|jpeg|png)
covers/cover-02.(jpg|jpeg|png)
covers/cover-03.(jpg|jpeg|png)
covers/cover-04.(jpg|jpeg|png)
covers/cover-05.(jpg|jpeg|png)
```

The image order must match `publish.md` title option order. For example, title option 1 maps to `cover-01`, title option 2 maps to `cover-02`, and so on.

Use a vertical social-friendly format by default, usually 9:16 for Douyin/Xiaohongshu-style publishing. If the target platform needs another ratio, note it in `brief.yaml`.

## publish.md

Include:

- at least 5 title options in different styles, such as direct, pain-point, method, contrarian, checklist, or curiosity-driven
- at least 5 description copy options, each no more than 30 Chinese characters
- caption/body for platform posting
- hashtags/topics
- pinned comment or comment prompt when useful
- CTA reply seed when the CTA asks for comments
- recommended publishing time windows for the target platform, with brief rationale and a note to adjust using account analytics
- manual publishing notes

## review.md

Use checkboxes. Include:

- facts checked or marked as needing checking
- no unsupported numbers, screenshots, quotes, or examples
- no exaggerated promises
- copyright/material authorization checked
- privacy checked
- AI-assisted labeling decision made
- article, topics, cover, and publish copy consistency checked
- cover image count and content align one-to-one with `publish.md` title options
- `publish.md` includes at least 5 description copy options, each no more than 30 Chinese characters
- recommended publishing time is suitable for the target platform and audience, without promising traffic results
- final human approval decision

## Completion Check

Before finishing, verify:

- All required text files exist.
- `article.md` does not include an article title; it includes summary, body, and references when needed; the summary is within 30 Chinese characters unless the user requested otherwise.
- `topics.md`, `cover.txt`, and `publish.md` are consistent with the article.
- `publish.md` includes at least 5 title options in different styles and at least 5 description copy options no longer than 30 Chinese characters each.
- `cover.txt` includes one cover prompt for each `publish.md` title option, with a minimum of 5 prompts.
- The same number of finished cover images exists under `covers/`, with a minimum of 5 images, or missing images are explicitly documented in `review.md`.
- `publish.md` includes recommended publishing time windows and frames them as testing guidance, not a performance guarantee.
- No final file contains empty template rows, blank required fields, or placeholder-only content.
