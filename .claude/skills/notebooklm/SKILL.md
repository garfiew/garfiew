---
name: notebooklm
description: Complete API for Google NotebookLM - full programmatic access including features not in the web UI. Create notebooks, add sources, generate all artifact types, download in multiple formats. Activates on explicit /notebooklm or intent like "create a podcast about X"
---

# NotebookLM Automation

Complete programmatic access to Google NotebookLM—including capabilities not exposed in the web UI. Create notebooks, add sources (URLs, YouTube, PDFs, audio, video, images), chat with content, generate all artifact types, and download results in multiple formats.

## Installation

**From PyPI (Recommended for AI agents — Python-version-aware):**
```bash
pip install "notebooklm-py[browser]"   # mandatory; errors must propagate

# [cookies] (rookiepy) is optional and known to FAIL TO BUILD on Python 3.13+.
# Skip it deliberately on 3.13+ rather than swallowing the error — that lets
# *real* install failures (typos, network, PyPI outages) surface for the agent.
if python -c "import sys; sys.exit(0 if sys.version_info < (3, 13) else 1)"; then
    pip install "notebooklm-py[cookies]"   # errors propagate
else
    echo "Skipping [cookies] on Python 3.13+ (rookiepy unavailable). Use 'notebooklm login' interactively."
fi
```

## Prerequisites

**IMPORTANT:** Before using any command, you MUST authenticate:

```bash
notebooklm login          # Opens browser for Google OAuth
notebooklm list           # Verify authentication works
```

## When This Skill Activates

**Explicit:** User says "/notebooklm", "use notebooklm", or mentions the tool by name

**Intent detection:** Recognize requests like:
- "Create a podcast about [topic]"
- "Summarize these URLs/documents"
- "Generate a quiz from my research"
- "Turn this into an audio overview"
- "Create flashcards for studying"
- "Generate a video explainer"
- "Make an infographic"
- "Create a mind map of the concepts"
- "Download the quiz as markdown"
- "Add these sources to NotebookLM"

## Autonomy Rules

**Run automatically (no confirmation):**
- `notebooklm status`, `notebooklm auth check`, `notebooklm list`
- `notebooklm source list`, `notebooklm artifact list`
- `notebooklm create`, `notebooklm source add`
- `notebooklm ask "..."` (without `--save-as-note`)

**Ask before running:**
- `notebooklm delete`, `source delete`, `note delete`, `artifact delete` — destructive
- `notebooklm generate *` — long-running, may fail
- `notebooklm download *` — writes to filesystem

## Quick Reference

| Task | Command |
|------|---------|
| Authenticate | `notebooklm login` |
| List notebooks | `notebooklm list` |
| Create notebook | `notebooklm create "Title"` |
| Set context | `notebooklm use <notebook_id>` |
| Add URL source | `notebooklm source add "https://..."` |
| Add file | `notebooklm source add ./file.pdf` |
| Chat | `notebooklm ask "question"` |
| Generate podcast | `notebooklm generate audio "instructions"` |
| Generate video | `notebooklm generate video "instructions"` |
| Generate report | `notebooklm generate report --format briefing-doc` |
| Generate quiz | `notebooklm generate quiz` |
| Check artifact status | `notebooklm artifact list` |
| Download audio | `notebooklm download audio ./output.mp3` |
| Download video | `notebooklm download video ./output.mp4` |
| Download report | `notebooklm download report ./report.md` |
| Health check | `notebooklm doctor` |

## Generation Types

| Type | Command | Download |
|------|---------|----------|
| Podcast | `generate audio` | .mp3 |
| Video | `generate video` | .mp4 |
| Slide Deck | `generate slide-deck` | .pdf / .pptx |
| Infographic | `generate infographic` | .png |
| Report | `generate report` | .md |
| Mind Map | `generate mind-map` | .json |
| Quiz | `generate quiz` | .json/.md/.html |
| Flashcards | `generate flashcards` | .json/.md/.html |

## Error Handling

| Error | Action |
|-------|--------|
| Auth/cookie error | `notebooklm auth check` then `notebooklm login` |
| "No notebook context" | Use `-n <id>` flag or `notebooklm use <id>` |
| Rate limiting | Wait 5-10 min, retry |
| Download fails | Check `artifact list` for status first |

## Processing Times

| Operation | Typical time |
|-----------|--------------|
| Source processing | 30s - 10 min |
| Quiz, flashcards | 5 - 15 min |
| Audio generation | 10 - 20 min |
| Video generation | 15 - 45 min |
