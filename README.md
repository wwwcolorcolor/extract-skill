# /extract

A Claude Code skill that extracts durable knowledge — frameworks, methodologies, mental models, systematic techniques — from any content source.

This is not summarization. This is extraction of transferable intelligence.

## Prerequisites

**Required for YouTube:**

```bash
# macOS
brew install yt-dlp

# or
pip install yt-dlp
```

**Optional:** For YouTube videos without captions (rare), audio transcription falls back to Groq Whisper:

```bash
export GROQ_API_KEY=your_key_here
```

Get one free at [console.groq.com](https://console.groq.com).

**Articles, local files, PDFs** — no dependencies needed.

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

Give it a URL or file path. It fetches the content, assesses quality, and extracts structured knowledge across these categories:

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

## Troubleshooting

**"yt-dlp: command not found"**
Install it: `brew install yt-dlp` or `pip install yt-dlp`

**YouTube extraction returns empty**
Some videos have no captions. Set `GROQ_API_KEY` for audio transcription fallback.

**WebFetch returns thin content for an article**
Some sites block automated fetching. Try `/extract` with a local copy of the text instead.
