# 试卷转刷题包 / Paper to Drill Pack

给 **Grok、Claude Code、Codex** 用的 skill：把 PDF、Word、JPEG/PNG 试卷转成家庭刷题实验室能直接用的 `packVersion: 1` JSON。

[English](#english)

## 用途

你手头有一份题目（能复制的 PDF/Word，或拍照/扫描的 JPEG），想放进 [家庭刷题实验室](https://github.com/fgyg007/family-drill-lab) 练习。这个 skill 规定：

- 输出哪一种 JSON（选择题 `mcq`，`packVersion: 1`）
- 阅读材料放 `passage`，题干放 `stem`
- **答案只能从卷面答案页抄**，不许编
- 残题、缺选项进 `quarantine`，不要硬塞进练习
- 没有答案的包是 `candidate`，进候选题包；过了门禁且答案齐全才进过关题包

练习页只读过关题包，不会直接打开 PDF。

## 安装

```bash
git clone https://github.com/fgyg007/paper-to-drill-pack-skill.git ~/.grok/skills/paper-to-drill-pack
git clone https://github.com/fgyg007/paper-to-drill-pack-skill.git ~/.claude/skills/paper-to-drill-pack
git clone https://github.com/fgyg007/paper-to-drill-pack-skill.git ~/.codex/skills/paper-to-drill-pack
```

网页聊天：把 [PASTE.md](PASTE.md) 全文贴进去，再附上 PDF / Word / 图片。

## 使用

```text
使用 $paper-to-drill-pack 把这份试卷转成刷题包 JSON。不要编答案。
```

标准字段见 [references/pack.schema.json](references/pack.schema.json)，完整样例见 [references/example.pack.json](references/example.pack.json)。规则正文是 [SKILL.md](SKILL.md)。

本机有刷题实验室时，转换后应跑：

```bash
cd /Users/jlmac/dev/projects/family-drill-lab
npm run validate:pack -- /path/to/pack.json
```

## 许可

[MIT](LICENSE)

---

## English

A skill for **Grok, Claude Code, and Codex**. It converts a PDF, Word, or JPEG/PNG paper into the JSON pack that [family-drill-lab](https://github.com/fgyg007/family-drill-lab) already loads.

It does **not** scrape a new product. It emits `packVersion: 1` multiple-choice items (`mcq` only for now).

Rules the agent must follow:

- Put shared reading text in `passage`, the question in `stem`
- Copy `answer` only from a printed answer key; never invent keys
- Incomplete items go in `quarantine`
- Packs without answers stay `candidate` (inbox), not `ready` (practice)
- Do not commit exam files or pack JSON to Git

### Install

```bash
git clone https://github.com/fgyg007/paper-to-drill-pack-skill.git ~/.grok/skills/paper-to-drill-pack
git clone https://github.com/fgyg007/paper-to-drill-pack-skill.git ~/.claude/skills/paper-to-drill-pack
git clone https://github.com/fgyg007/paper-to-drill-pack-skill.git ~/.codex/skills/paper-to-drill-pack
```

For a browser chat, paste [PASTE.md](PASTE.md), then attach the paper.

### Use

```text
Use $paper-to-drill-pack to convert this paper into family-drill pack JSON. Do not invent answers.
```

### License

[MIT](LICENSE)
