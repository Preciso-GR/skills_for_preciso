# Contributing to Skills for Preciso

Thanks for your interest in contributing.
This repo is the skill library for Preciso — it's early-stage
and growing, and new domain skills are the highest-value
contribution you can make.

---

## What We Need Most

### New Extraction Skills

A skill is a markdown file that tells agents how to read a
specific document type and turn it into Preciso's extraction
JSON (entities + relationships + chunks).

Skills we want:
  - Legal contracts (parties, obligations, dates, penalties)
  - Medical/clinical documents (conditions, treatments, outcomes)
  - ESG reports (metrics, commitments, targets)
  - Patent filings (claims, prior art, inventors)
  - News articles (events, entities, sentiment)
  - Earnings call transcripts (guidance, tone, Q&A)
  - Support tickets / customer conversations
  - Changelogs / release notes

How to contribute a skill:
  1. Fork the repo
  2. Copy General-graph-extraction-skill/SKILL.md as your starting point
  3. Create a new folder: Your-Domain-Skill/SKILL.md
  4. Write frontmatter (`name`, `description`) — see "Making Your Skill
     Routable" below
  5. Adapt the entity types and relationship types for your domain
  6. Add 2-3 extraction examples showing input text and expected output JSON
  7. Test it against a real document with a running Preciso instance
  8. Open a PR with your test results

### Eval Results on New Skills or Documents

If you run the `evals/` skill against a graph built from your
new skill (or an existing one on a new document type), open a
PR with the results. This builds the shared benchmark set.

### Bug Reports

Open an issue with:
  - Which skill you used
  - The document type you fed it
  - What extraction you expected vs. what you got

---

## Making Your Skill Routable

Agents choose a skill by matching a document against every
`SKILL.md`'s frontmatter `description` — there's no separate
registry, so the frontmatter *is* the routing logic. A good
description:

  - names the concrete document types/triggers it should fire on
    ("10-K or 10-Q filings", "meeting notes or tickets")
  - includes a few natural-language phrasings a user might say
    ("map dependencies in my codebase", "extract for graph-rag")
  - explicitly says which existing skill to use instead when the
    document is actually that skill's domain (avoids two skills
    both claiming the same document)
  - ends with the schema requirement: output must follow the
    GraphRAG ingestion schema (`document_id` + `entities` +
    `relationships` + `chunks`, chunk-based `source_id` values)

Look at any existing `SKILL.md` frontmatter for the pattern before
writing your own.

---

## How to Open a PR

1. Fork the repo
2. Create a branch: `git checkout -b your-skill-name`
3. Add or edit your `SKILL.md`
4. Test it: point an agent with a running Preciso MCP server at a
   real document of your target type, run the skill end-to-end
   (extraction → ingestion → a few queries), and confirm the
   ingestion succeeds with no orphaned references
5. Open a PR with a clear description

---

## Skill Contribution Template

When contributing a new skill, your PR description should include:

  Domain: [what document type]
  Entity types added: [list]
  Relationship types added: [list]
  Test document used: [describe it, do not attach if confidential]
  Sample queries tried: [2-3 natural-language questions the graph answered]
  Eval result (if run): [overall score]

---

## What We Will Not Merge

  - Skills without at least one worked extraction example (input text + output JSON)
  - Skills whose frontmatter description overlaps an existing skill's
    domain without explaining which one an agent should pick
  - Skills that require calling ingest tools directly from a
    reconciliation/patch role (reconciliation skills only ever emit
    patch files)
  - Output schemas that don't conform to Preciso's required
    `document_id` / `entities` / `relationships` / `chunks` contract

---

## Community

  GitHub Discussions for questions and ideas
  Issues for bugs and feature requests
  PRs for new skills and eval results

Part of the [Preciso](https://github.com/Preciso-GR/preciso-graphrag) project.
Named after Bruno Fernandes. Every contribution should land exactly where it needs to.
