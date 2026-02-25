# /extract

A Claude Code skill that extracts durable knowledge — frameworks, methodologies, mental models, systematic techniques — from any content source.

This is not summarization. This is extraction of transferable intelligence.

## Prerequisites

This skill depends on the [`summarize`](https://github.com/steipete/summarize) CLI for content fetching:

```bash
# Requires Node.js 22+
npm install -g @steipete/summarize
```

**Optional:** For audio/video transcription (YouTube without captions, podcasts), you need a Groq API key:

```bash
export GROQ_API_KEY=your_key_here
```

Get one free at [console.groq.com](https://console.groq.com). Not needed for articles, web pages, or YouTube videos with captions.

**Optional:** For YouTube videos where captions aren't available:

```bash
# macOS
brew install yt-dlp

# or
pip install yt-dlp
```

## Install

```bash
claude skill add --url https://github.com/wwwcolorcolor/extract-skill
```

Verify it installed:

```bash
claude skill list
```

You should see `extract` in the output.

## Usage

```
/extract <url>           — extract from a YouTube video, article, or podcast
/extract <file_path>     — extract from a local file
```

### Examples

```
/extract https://www.youtube.com/watch?v=dQw4w9WgXcQ
/extract https://paulgraham.com/greatwork.html
/extract ./transcript.txt
```

## What it does

Give it a URL or file path. It pulls the raw content, assesses quality, and extracts structured knowledge across these categories:

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

**"summarize: command not found"**
Install it: `npm install -g @steipete/summarize`

**YouTube extraction fails**
Try installing yt-dlp (`brew install yt-dlp`) — some videos need it as a fallback.

**Empty output from audio/video**
You probably need a `GROQ_API_KEY` for transcription. Get one at [console.groq.com](https://console.groq.com).
