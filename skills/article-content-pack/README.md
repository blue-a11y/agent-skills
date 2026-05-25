# article-content-pack

Codex skill for creating publish-ready Chinese article content packs.

It generates:

- `brief.yaml`
- `article.md`
- `topics.md`
- `cover.txt`
- at least five cover images under `covers/`, aligned one-to-one with publish title options
- `publish.md`
- `review.md`

The skill is designed for Chinese article workflows such as WeChat official account articles, Douyin image-text posts, Xiaohongshu-style posts, technical articles, cover briefs, publishing copy, topic tags, and review checklists.

## Install

Copy this folder into your Codex skills directory:

```sh
mkdir -p ~/.codex/skills
cp -R article-content-pack ~/.codex/skills/
```

Then ask Codex to use `article-content-pack`.

## Notes

Generated article packages should not be written inside this skill directory. The default output base directory is `~/Desktop/artcle-create`, unless configured otherwise by the skill.
