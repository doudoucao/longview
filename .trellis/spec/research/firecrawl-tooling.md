# Firecrawl Tooling

## Purpose

Use Firecrawl as the default research acquisition tool for Longview agent research tasks. Firecrawl should be used for web search, page scraping, local file parsing, site crawling, arXiv/GitHub research, and browser interaction when regular scraping is insufficient.

## Authentication

Firecrawl reads credentials from:

- `FIRECRAWL_API_KEY`
- optional `FIRECRAWL_API_URL` for self-hosted deployments

Keep credentials in `.env`, shell environment variables, or a secret manager. Never hardcode API keys or commit `.env`.

Useful checks:

```bash
firecrawl --status
firecrawl view-config
```

## Default Output Locations

For task research, save Firecrawl outputs under the active task's `sources/` directory:

```text
.trellis/tasks/<task>/sources/
  index.md
  raw/
  notes/
  extracts/
```

Use `/tmp` only for throwaway smoke tests. Do not commit `.firecrawl/` cache or `.env`.

## Core Commands

### Search

Use when the agent does not know the exact URL yet.

```bash
firecrawl search "agentic investment research tools GitHub" \
  --limit 10 \
  --json \
  -o .trellis/tasks/<task>/sources/raw/search-agentic-investment.json
```

Use `--scrape` when search results should include full markdown content:

```bash
firecrawl search "LLM agents financial research workflow" \
  --limit 5 \
  --scrape \
  --scrape-formats markdown \
  --json \
  -o .trellis/tasks/<task>/sources/raw/search-llm-agents-finance-scraped.json
```

### Scrape

Use when the exact URL is known.

```bash
firecrawl scrape "https://example.com/research-page" \
  --only-main-content \
  -o .trellis/tasks/<task>/sources/raw/example-research-page.md
```

Use multiple formats when links or structured artifacts matter:

```bash
firecrawl scrape "https://example.com/docs" \
  --format markdown,links \
  --only-main-content \
  -o .trellis/tasks/<task>/sources/raw/example-docs.json
```

### Parse

Use for local files such as PDFs, DOCX, HTML, XLSX, and similar research documents.

```bash
firecrawl parse ./paper.pdf \
  -f markdown,links \
  --json \
  --pretty \
  -o .trellis/tasks/<task>/sources/extracts/paper-parsed.json
```

Ask targeted questions only when useful:

```bash
firecrawl parse ./report.pdf \
  -Q "What data sources and workflow steps does this report describe?"
```

### Crawl

Use for many pages under one site or docs section.

```bash
firecrawl crawl "https://example.com/docs" \
  -o .trellis/tasks/<task>/sources/raw/example-docs-crawl.json
```

### Map

Use to discover URLs before deciding what to scrape or crawl.

```bash
firecrawl map "https://example.com" \
  -o .trellis/tasks/<task>/sources/raw/example-map.json
```

### Research

Use for arXiv paper discovery and GitHub issue/PR/README history.

```bash
firecrawl research search-papers "LLM agents for financial research" --limit 20
firecrawl research inspect-paper arxiv:1706.03762
firecrawl research read-paper arxiv:1706.03762 --question "What agent workflow is proposed?"
firecrawl research search-github "investment research agent workflow" --limit 20
```

### Interact

Use only when search/scrape/crawl cannot access required content because interaction is needed.

```bash
firecrawl interact "Open the page, click pricing, and extract the plan table"
```

### Agent

Use for structured extraction tasks where the browser agent can navigate and return data.

```bash
firecrawl agent "Find the pricing and API limits for this data provider and return a concise source-backed summary"
```

## Workflow Escalation

Prefer this order:

1. `search` to discover sources.
2. `scrape` known high-value pages.
3. `map` to discover site structure.
4. `crawl` a docs section or site subset.
5. `parse` local PDFs/files.
6. `research search-papers` / `research search-github` for papers and GitHub.
7. `interact` or `agent` only when static extraction is not enough.

## Source Notes

After using Firecrawl, create or update `sources/index.md` and source notes. Record:

- command used
- output file path
- source URL or local file
- access date
- source type
- saved form
- key claims or capabilities
- limitations, paywall/copyright/access issues
- relevance to Longview

## Safety

- Do not print or commit `FIRECRAWL_API_KEY`.
- Do not commit `.env`.
- Do not commit `.firecrawl/` cache unless a task explicitly requires curated artifacts from it.
- Respect paywalls, robots, account restrictions, copyright limits, and terms of service.
- Prefer saving links, metadata, short excerpts, and source notes when full raw content should not be stored.
