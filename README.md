# /extract

A Claude Code skill that extracts durable knowledge — frameworks, methodologies, mental models, systematic techniques — from any content source.

This is not summarization. This is extraction of transferable intelligence.

## What it does

Give it a URL (YouTube video, article, podcast) or file path. It pulls the raw content, assesses quality, and extracts structured knowledge across these categories:

- **Mental Models & Frameworks** — decision-making heuristics, expert lenses
- **Systematic Methods & Processes** — step-by-step techniques, playbooks, workflows
- **Use Cases & Applications** — concrete real-world examples and results
- **Principles & Heuristics** — transferable rules of thumb
- **Contrarian & Non-Obvious Insights** — challenges to conventional wisdom
- **Specific Techniques & Tactics** — immediately applicable, named methods

Shallow content gets called out. Depth of extraction matches depth of source.

## Install

```bash
claude skill add --url https://github.com/wwwcolorcolor/extract
```

## Usage

```
/extract <url>           — extract from a YouTube video, article, or podcast
/extract <file_path>     — extract from a local file
```

## What gets stripped

- Sponsored segments, ad reads, self-promotion
- Time-sensitive noise (launch announcements, pricing, waitlist status)
- Capability comparisons that expire

## What gets preserved

- Reasoning behind tool/approach choices (selection criteria outlive specific tools)
- Prediction reasoning chains
- Historical context explaining why something works the way it does
