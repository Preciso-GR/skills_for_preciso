# Skills for Preciso

A community repo of **extraction skills** for [Preciso](https://github.com/Preciso-GR/preciso-graphrag) — a local-first GraphRAG system. A skill is a markdown file that teaches an agent (Claude Code, Codex, Copilot, OpenCode, etc.) how to read a specific *type* of document and turn it into graph extraction JSON that Preciso can ingest.

This repo is not Preciso itself — it holds no code, no MCP server, no graph storage. It's the skill library that plugs into `preciso-graphrag/skills/` (or any other Preciso checkout) and grows with contributions for new document types and use cases.

## Why a separate repo

Skills are content, not code — they change on a different cadence than the ingestion engine, and different use cases (financial, research, general, reconciliation, evals, and whatever comes next) each want their own reviewers and iteration speed. Keeping them here means:

- anyone can add a skill for a new domain without touching `preciso-graphrag`
- skills can be versioned and reviewed independently of the core engine
- the same skill library can be dropped into any Preciso checkout

## Skills in this repo

| Skill | Path | Use When |
|-------|------|----------|
| Financial | `Financial-Graph-Extraction/SKILL.md` | 10-Ks, 10-Qs, earnings calls, analyst reports — high-precision, numerically-grounded extraction |
| Research | `Research-paper-graph-extraction-skill/SKILL.md` | Research papers, citation networks, multi-paper corpora |
| General | `General-graph-extraction-skill/SKILL.md` | Codebases, wikis, READMEs, meeting notes, tickets, general text |
| Reconciliation | `Reconciliation-Subagent-Skill/SKILL.md` | Cleans up an *existing* extraction JSON only — never touches raw source docs, never calls ingest tools |
| Evals | `evals/SKILL.md` | Agent-based evaluation of a built graph — generates questions, queries the graph, scores results. Run only after ingestion |

An agent picks the right skill by matching its `SKILL.md` frontmatter `description` against the document at hand — see [CONTRIBUTING.md](CONTRIBUTING.md) for what makes a description routable.

## Using a skill

1. Drop this repo's contents into a Preciso checkout's `skills/` folder (or point your agent at this repo directly).
2. Ask your agent to process a document; it reads the `SKILL.md` frontmatter descriptions to pick the right one.
3. The skill walks the agent through reading the source, chunking it, extracting entities/relationships into `extractions/{source_name}_extracted.json`, and calling the Preciso MCP ingest tool.

Every skill's output must conform to Preciso's ingestion schema: `document_id` + `entities` + `relationships` + `chunks`, with every entity/relationship `source_id` pointing at a real `chunk_id`. See any existing `SKILL.md` (e.g. `General-graph-extraction-skill/SKILL.md`) for the full schema and worked examples.

## Evals

`evals/` holds sample questions and evaluation assets for smoke-testing a graph after ingestion:

- `evals/WALMART_SAMPLE_QUESTIONS.json` — a curated set of 23 sample questions over the Walmart 2022/2023 extraction artifacts; safe to commit, used as an immediate smoke test.
- `evals/results/` and `evals/test_cases/` — runtime-generated run outputs and generated test suites. Not committed (see `.gitignore`).

## Contributing

Want to add a skill for a new domain (legal contracts, support tickets, product specs, changelogs, whatever you're graphing)? See [CONTRIBUTING.md](CONTRIBUTING.md).
