# winning-pitch-deck

A Claude skill that helps founders craft or review a 3-minute pitch deck and speaker script using the **DePitch framework** by [Sofian](https://x.com/depitchsofian) / [DePitch Academy](https://x.com/depitch_academy).

Built for the [Colosseum Frontier](https://www.colosseum.org/frontier) Solana hackathon (3-min pitch video + 3-min demo video), but the framework applies to any short founder pitch — demo day, YC, investor meeting, startup competition.

**Credibility:** the framework has shipped 16 Colosseum wins, 3 accelerator entries, and $2.5M+ raised across teams that used it.

## What the skill does

Two modes, auto-detected:

- **Build mode** — you have an idea or product, no deck yet. The skill runs a structured founder interview, drafts a slide-by-slide outline, writes a speaker script with timing marks, and gives you a rehearsal checklist.
- **Review mode** — you have a deck, script, or pitch-video transcript. The skill scores it against a rubric, tags failure patterns, and returns the top 3–5 prioritized fixes — not a full rewrite.

It enforces the non-negotiables: tense discipline (no "trying to"/"hope to"), demo before 2:00, traction with real metrics (no X followers), business-model slide, CTA instead of "thank you", one memorable line embedded.

## Install

### Claude Code

```bash
git clone https://github.com/0xbenbottle/winning-pitch-deck.git \
  ~/.claude/skills/winning-pitch-deck
```

Restart Claude Code. The skill is available to all your sessions.

### Claude.ai

Package the skill into a `.skill` file and upload it via your workspace's skill settings. If you have `skill-creator` installed locally:

```bash
python -m scripts.package_skill /path/to/winning-pitch-deck
```

### Other runtimes (Copilot CLI, Gemini CLI, Codex)

See each platform's skill-install docs — the skill uses Claude Code tool names, but the content is platform-neutral.

## Usage

Once installed, the skill triggers when you mention pitch work:

> "help me build a 3-minute pitch for the Colosseum Frontier submission"
>
> "roast my deck — the problem slide feels weak"
>
> "my cofounder's script is full of 'we're trying to' — fix the tense"

If it doesn't auto-trigger (some queries are handled without consulting skills), invoke it explicitly:

> "use the winning-pitch-deck skill to review this transcript"

## Skill contents

```
winning-pitch-deck/
├── SKILL.md                              # framework, workflow, hard rules
├── references/
│   ├── slide-by-slide.md                 # per-slide guide with winning examples
│   ├── delivery-10-techniques.md         # delivery technique checklist
│   ├── failure-patterns.md               # 5 pitch-killer patterns
│   ├── founder-interview.md              # structured interview for build mode
│   └── review-rubric.md                  # scoring rubric for review mode
└── assets/
    └── deck-outline-template.md          # fillable outline with timing marks
```

## Credits

The framework, examples, and delivery techniques come from Sofian's [DePitch Academy](https://x.com/depitch_academy) workshop *"Crafting a winning pitch for Colosseum Frontier"*, hosted by [Superteam Canada](https://superteam.ca/) in April 2026. Winning-deck examples referenced: Surf Cache, Nomu, Spend, Crypto Fantasy League, Loot Go.

Packaged as a Claude skill by [BenBottle](https://github.com/0xbenbottle).

## Contributing

Spotted something off, want to add another winning example, or have a framework improvement? Open an issue or PR.

## License

MIT — see `LICENSE`.
