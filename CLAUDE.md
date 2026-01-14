# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Claude Code marketplace plugin providing AI-powered content generation skills. Skills use Gemini Web API (reverse-engineered) for text/image generation and Chrome CDP for browser automation.

## Architecture

```
skills/
├── gemini-web/          # Core: Gemini API wrapper (text + image gen)
├── xhs-images/          # Xiaohongshu infographic series (1-10 images)
├── cover-image/         # Article cover images (2.35:1 aspect)
├── slide-deck/          # Presentation slides with outlines
├── article-illustrator/ # Smart illustration placement
└── post-to-x/           # X/Twitter posting automation
```

Each skill contains:
- `SKILL.md` - YAML front matter (name, description) + documentation
- `scripts/` - TypeScript implementations
- `prompts/system.md` - AI generation guidelines (optional)

## Running Skills

All scripts run via Bun (no build step):

```bash
npx -y bun skills/<skill>/scripts/main.ts [options]
```

Examples:
```bash
# Text generation
npx -y bun skills/gemini-web/scripts/main.ts "Hello"

# Image generation
npx -y bun skills/gemini-web/scripts/main.ts --prompt "A cat" --image cat.png

# From prompt files
npx -y bun skills/gemini-web/scripts/main.ts --promptfiles system.md content.md --image out.png
```

## Key Dependencies

- **Bun**: TypeScript runtime (via `npx -y bun`)
- **Chrome**: Required for `gemini-web` auth and `post-to-x` automation
- **No npm packages**: Self-contained TypeScript, no external dependencies

## Authentication

`gemini-web` uses browser cookies for Google auth:
- First run opens Chrome for login
- Cookies cached in data directory
- Force refresh: `--login` flag

## Plugin Configuration

`.claude-plugin/marketplace.json` defines plugin metadata and skill paths. Version follows semver.

## Adding New Skills

1. Create `skills/<name>/SKILL.md` with YAML front matter
2. Add TypeScript in `skills/<name>/scripts/`
3. Add prompt templates in `skills/<name>/prompts/` if needed
4. Register in `marketplace.json` plugins[0].skills array

## Code Style

- TypeScript throughout, no comments
- Async/await patterns
- Short variable names
- Type-safe interfaces
