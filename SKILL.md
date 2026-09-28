---
name: paper-to-drill-pack
description: >
  Use when converting a PDF, Word, JPEG, PNG, or scanned exam paper into a
  family-drill-lab pack JSON; when the user says 转成题包, 导入刷题库, packVersion,
  过关题包, 候选题包, 试卷转JSON, or drops a paper to make practice questions.
---

# Paper → family drill pack

Turn a paper (PDF / Word / JPEG / PNG) into `packVersion: 1` JSON that 家庭刷题实验室 can load. Do not invent a new format. Do not write a website. Do not guess answers.

Live gate in the app: `family-drill-lab/src/pack/validate.mjs`. Machine schema: `references/pack.schema.json`. Example: `references/example.pack.json`.

## Output

One JSON object, UTF-8, `.json` file. First period: **only `mcq`**.

```json
{
  "packVersion": 1,
  "id": "kitchen-paper-v1",
  "title": "Kitchen five-page paper",
  "locale": "en",
  "status": "candidate",
  "problems": [],
  "quarantine": []
}
```

| Field | Rule |
|---|---|
| `packVersion` | Must be `1` |
| `id` | `^[a-z0-9]+(?:-[a-z0-9]+)*$`, 2–64 chars |
| `status` | `candidate` unless every problem is complete **and** every answer was copied from a visible answer key |
| `problems` | Complete `mcq` items only |
| `quarantine` | Incomplete stems, cut-off choices, answer-key pages, unknown types |

### Each problem

| Field | Rule |
|---|---|
| `id` | Unique in pack; `{pack-id}-p{page}-q{n}` |
| `type` | `mcq` only. `true_false` / `fill` / `listening_mcq` → quarantine `type_not_enabled` |
| `subject` | Required. Ask if unknown (`cet4-reading`, `zhongkao-math`, …) |
| `stem` | Question only. Do not dump the passage here |
| `passage` | Shared reading text, same string on every item in the group |
| `choices` | 2–8 `{ "id", "text" }`. Keep printed ids (`A`/`B`/`C`/`D`). Do not invent missing options |
| `answer` | Must equal a choice `id`. **Only if the paper shows the key.** Otherwise omit `answer` and keep `status: "candidate"` |
| `source` | `{ "kind": "exam" \| "imported" \| "original", "title": "...", "locator": "p1-q1" }` |

## Conversion steps

1. Read the file. PDF/Word: extract text. JPEG/PNG: read the image. Unreadable glyphs → quarantine, do not guess.
2. Split into passages and numbered items. An `ANSWER KEY` / `参考答案` page is **not** a problem; use it only to fill `answer`.
3. An item missing a full choice set, stem, or type goes to `quarantine` with blockers (`missing_choices`, `incomplete_choice_set`, `missing_stem`).
4. Copy answers only from the printed key. No key → no `answer`. Never batch-generate keys.
5. Write JSON. If this machine has the lab repo, run:

```bash
cd /Users/jlmac/dev/projects/family-drill-lab
npm run validate:pack -- /path/to/pack.json
```

6. Save:
   - No answers or any quarantine → Notes `家庭刷题实验室/项目数据/候选题包/<id>.json`
   - Gate `ok` and every item has a key-copied answer → may go to `项目数据/过关题包/<id>.json`
   - Never commit papers or pack JSON to Git

If Notes is unmounted, give the user the JSON and the target path. Do not write secrets.

## Do not

- Invent stems, choices, or answers
- Put the passage inside `stem`
- Mark `status: "ready"` when answers were guessed
- Enable non-`mcq` types
- Push exam PDFs, photos, or question JSON to GitHub

## After conversion

Tell the user: pack id, ready count, quarantine count, file path, and that practice at http://localhost:3120 only loads **过关** packs.
