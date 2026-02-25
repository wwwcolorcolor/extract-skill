---
name: extract
description: >
  Extract knowledge, frameworks, and methodologies from any URL or content.
  Use when: (1) user says "/extract", "extract this", "extract from",
  (2) user shares a URL or file and wants the key insights pulled out,
  (3) user wants to learn from a video, article, or podcast without reading/watching the whole thing.
  NOT for: summarization, news digests, or content that doesn't contain transferable knowledge.
  Requires: yt-dlp (for YouTube).
user_invocable: true
trigger: /extract
arguments:
  - name: source
    description: URL (YouTube, article) or file path to extract from
    required: true
---

# /extract — Knowledge Extraction Skill

Extract durable knowledge — frameworks, methodologies, mental models, systematic techniques — from any content source. This is NOT summarization. This is extraction of transferable intelligence.

## Workflow

### Step 1: Identify source type and fetch content

Detect the source type and use the appropriate method:

**YouTube video** (URL contains `youtube.com` or `youtu.be`):
Extract the video ID from the URL, then fetch subtitles with yt-dlp:

```bash
yt-dlp --write-auto-sub --sub-lang "en" --skip-download --sub-format vtt -o "/tmp/yt-%(id)s" "<url>" 2>/dev/null && cat /tmp/yt-*<video_id>*.vtt 2>/dev/null
```

If that fails (no captions available), download audio and note that transcription isn't available:

```bash
yt-dlp -x --audio-format mp3 -o "/tmp/yt-%(id)s.%(ext)s" "<url>" 2>/dev/null
```

Then transcribe via Groq Whisper if user has `GROQ_API_KEY` set:

```bash
curl -s https://api.groq.com/openai/v1/audio/transcriptions \
  -H "Authorization: Bearer $GROQ_API_KEY" \
  -F "file=@/tmp/yt-<video_id>.mp3" \
  -F "model=whisper-large-v3-turbo"
```

If neither path works, tell the user and stop.

Clean up temp files after extraction:

```bash
rm -f /tmp/yt-<video_id>* 2>/dev/null
```

**Web article / blog / page** (any HTTP/HTTPS URL that is not YouTube):
Use the `WebFetch` tool with the prompt: "Return the complete text content of this page. Preserve all details, quotes, examples, and structure. Do not summarize."

**Local file** (file path):
Use the `Read` tool to read the file directly.

**PDF file** (`.pdf` path):
Use the `Read` tool with the `pages` parameter if large.

If fetching fails or returns empty, tell the user what failed and stop. Don't guess.

### Step 2: Assess content quality

Before extracting, quickly scan the raw content. If the source is:
- **Shallow** (listicle, hype piece, promotional fluff, no real methodology) — say so in 1-2 lines. Don't manufacture depth that isn't there.
- **News/announcement-only** (product launch, model release, funding round) — say so. Extract only if there's actual methodology buried in the announcement.
- **Substantial** — proceed to extraction.

### Step 3: Extract knowledge

Apply the extraction framework below to the raw content. Be thorough. Preserve specificity — keep concrete examples, exact numbers, named techniques, and step-by-step processes. Do not abstract away the details that make knowledge actionable.

## Extraction Framework

Organize extracted knowledge into whichever of these categories are present. Skip empty categories — don't force content into categories where it doesn't belong.

### Mental Models & Frameworks
Ways of thinking about problems. Decision-making heuristics. Lenses for understanding systems. How experts frame situations differently than novices.

### Systematic Methods & Processes
Step-by-step techniques. Playbooks. Workflows. Repeatable processes someone could follow. Include the sequence and the reasoning behind each step.

### Use Cases & Applications
Concrete examples of how something works in practice. Real scenarios, not hypotheticals. What was the situation, what was done, what was the result.

### Principles & Heuristics
Underlying truths. Rules of thumb. "Always X, never Y" type guidance. The kind of knowledge that transfers across domains.

### Contrarian & Non-Obvious Insights
Things that challenge conventional wisdom. Counterintuitive findings. "Most people think X, but actually Y because Z." Only include if genuinely non-obvious — don't stretch.

### Specific Techniques & Tactics
Named techniques, tools-as-methodology (not tool announcements), configuration patterns, prompt structures, negotiation moves, etc. The concrete, immediately applicable stuff.

## Filtering Rules — STRICT

**Always strip:**
- Sponsored segments, ad reads, brand promotions, discount codes, CTAs
- "This video is brought to you by..." — pretend it doesn't exist
- Self-promotion ("subscribe", "like and comment", "check out my course")
- Filler intros/outros with no content

**Always strip time-sensitive noise:**
- "[Tool/model/product] just launched/released/announced" — unless the methodology of HOW it was built or WHY certain decisions were made is the actual insight
- Pricing, availability dates, waitlist status
- "Breaking news" framing
- Comparisons that are purely about current capabilities (these expire)

**Always preserve:**
- The REASONING behind why someone chose a tool or approach (even if the specific tool is ephemeral, the selection criteria are durable)
- Predictions with clear reasoning chains (the reasoning is the value, not the prediction)
- Historical context that explains WHY something works the way it does

## Output Format

Start with a one-line label:

```
**Source:** [title if available] — [URL]
```

Then the extracted knowledge organized by the categories above. Use markdown headers (`###`) for categories.

Within each category:
- Use bullet points for discrete insights
- Use numbered lists for sequential processes/steps
- Use blockquotes (`>`) for particularly sharp direct quotes worth preserving verbatim
- Bold key terms and technique names on first mention

Keep output as long as the content warrants. A 2-hour podcast might produce 2000+ words of extraction. A shallow 5-minute video might produce 3 lines. Match the depth of extraction to the depth of the source.

## Edge Cases

- **Multiple topics in one source**: Extract all of them. Use `---` horizontal rules to separate distinct topic clusters if they're unrelated.
- **Interview/conversation format**: Extract from ALL participants, not just the host or the most prominent speaker.
- **Tutorial/how-to content**: Preserve the full process, don't skip steps. This is exactly the kind of systematic method we want.
- **Non-English content**: Extract in English. Preserve original terms in parentheses when they don't translate cleanly.
