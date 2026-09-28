# Convert a paper into a family-drill pack (paste into any AI)

You convert PDF / Word / JPEG / PNG exam papers into JSON for 家庭刷题实验室. Do not invent answers. Do not build a new app.

## Format (`packVersion: 1`)

```json
{
  "packVersion": 1,
  "id": "my-paper-v1",
  "title": "Paper title",
  "locale": "zh-CN",
  "status": "candidate",
  "problems": [
    {
      "id": "my-paper-v1-p1-q1",
      "type": "mcq",
      "subject": "cet4-reading",
      "passage": "Shared reading text or omit",
      "stem": "Question only",
      "choices": [
        { "id": "A", "text": "..." },
        { "id": "B", "text": "..." },
        { "id": "C", "text": "..." },
        { "id": "D", "text": "..." }
      ],
      "answer": "B",
      "source": { "kind": "imported", "title": "Paper title", "locator": "p1-q1" }
    }
  ],
  "quarantine": []
}
```

Rules:

- First period: `type` must be `mcq`. 2–8 choices.
- `id` values: lowercase letters, digits, hyphens.
- Reading text goes in `passage`, not `stem`.
- `answer` only if the paper prints a key; it must match a choice id. Otherwise omit `answer` and keep `status: "candidate"`.
- Incomplete items go in `quarantine`. Answer-key pages are not problems.
- Do not guess unreadable scan text.
- Save to Notes `家庭刷题实验室/项目数据/候选题包/` unless every item is complete with a copied key — then `过关题包/`. Never git-commit papers or packs.

If `family-drill-lab` is on this machine: `npm run validate:pack -- the.json`.
