# /extract

A Claude Code skill that extracts durable knowledge — frameworks, methodologies, mental models, systematic techniques — from any content source.

This is not summarization. This is extraction of transferable intelligence.

## Prerequisites

None. This skill uses Claude Code's built-in tools only — no external dependencies.

For YouTube transcript extraction, the [youtube-transcript MCP server](https://github.com/nicobailon/mcp-youtube-transcript) is recommended but optional. Install it via Claude Code's MCP settings if you want YouTube support.

## Install

```bash
claude skill add --url https://github.com/wwwcolorcolor/extract-skill
```

Verify it installed:

```bash
claude skill list
```

## Usage

```
/extract <url>           — extract from a YouTube video or article
/extract <file_path>     — extract from a local file or PDF
```

### Examples

```
/extract https://www.youtube.com/watch?v=dQw4w9WgXcQ
/extract https://paulgraham.com/greatwork.html
/extract ./transcript.txt
/extract ./research-paper.pdf
```

## What it does

Give it a URL or file path. It fetches the content using built-in tools, assesses quality, and extracts structured knowledge across these categories:

- **Mental Models & Frameworks** — decision-making heuristics, expert lenses
- **Systematic Methods & Processes** — step-by-step techniques, playbooks, workflows
- **Use Cases & Applications** — concrete real-world examples and results
- **Principles & Heuristics** — transferable rules of thumb
- **Contrarian & Non-Obvious Insights** — challenges to conventional wisdom
- **Specific Techniques & Tactics** — immediately applicable, named methods

Shallow content gets called out. Depth of extraction matches depth of source.

## What gets stripped

- Sponsored segments, ad reads, self-promotion
- Time-sensitive noise (launch announcements, pricing, waitlist status)
- Capability comparisons that expire

## What gets preserved

- Reasoning behind tool/approach choices (selection criteria outlive specific tools)
- Prediction reasoning chains
- Historical context explaining why something works the way it does
