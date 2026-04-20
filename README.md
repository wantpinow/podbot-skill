# podbot-skill

Claude Code skill for the [`podbot`](https://www.npmjs.com/package/podbot) CLI — semantic search over a curated index of tech, VC, and business podcasts.

This repo is a public mirror of the `SKILL.md` that ships inside the `podbot` npm package. It exists so the skill can be installed with `npx skills` into any agent harness (Claude Code, Codex, Cursor, etc.), not just Claude Code.

## Install

### Via the [`skills`](https://github.com/vercel-labs/skills) CLI (recommended)

```bash
npx skills add wantpinow/podbot-skill
```

### Via the podbot CLI itself (Claude Code only)

If you're using Claude Code, the skill is bundled with the CLI:

```bash
npm install -g podbot
podbot install-skill
```

This writes the same `SKILL.md` to `~/.claude/skills/podbot/SKILL.md`.

## What the skill does

The skill teaches your agent when and how to use the `podbot` CLI for podcast research — including command structure, semantic search strategy, query patterns, and how to cite findings. See [`skills/podbot/SKILL.md`](skills/podbot/SKILL.md) for the full content.

## The podbot CLI

The CLI itself lives in a separate repo and is published to npm:

- **npm package**: https://www.npmjs.com/package/podbot
- **Install**: `npm install -g podbot`

## License

MIT
