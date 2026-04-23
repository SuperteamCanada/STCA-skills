# STCA Skills

A curated collection of agentic skills for founders — maintained by [Superteam Canada](https://superteam.ca/).

Each skill encodes a specific workflow, framework, or playbook so any founder with a Claude-powered assistant (Claude Code, Claude.ai, Copilot CLI, Gemini CLI, etc.) can tap into expert-level guidance on demand.

## Available skills

| Skill | What it does |
|---|---|
| [winning-pitch-deck](./skills/winning-pitch-deck/) | Craft or review a 3-minute founder pitch deck and speaker script using the DePitch framework. Built for Colosseum Frontier; works for any short investor/judge pitch. |

More skills coming — security review, product review, idea validation, hackathon submission prep, and others requested by the community.

## Install a skill

### Claude Code

Clone only the skill you want:

```bash
git clone --depth 1 --filter=blob:none --sparse \
  https://github.com/SuperteamCanada/STCA-skills.git \
  ~/.claude/skills/_stca-tmp
cd ~/.claude/skills/_stca-tmp
git sparse-checkout set skills/winning-pitch-deck
mv skills/winning-pitch-deck ~/.claude/skills/winning-pitch-deck
cd .. && rm -rf _stca-tmp
```

Or clone the whole collection and symlink what you want:

```bash
git clone https://github.com/SuperteamCanada/STCA-skills.git ~/code/STCA-skills
ln -s ~/code/STCA-skills/skills/winning-pitch-deck ~/.claude/skills/winning-pitch-deck
```

Restart Claude Code. The skill is available in all your sessions.

### Claude.ai

Package the skill folder into a `.skill` file (using Anthropic's `skill-creator` locally) and upload via your workspace's skill settings.

### Other runtimes

Each platform has its own install path — check the platform docs.

## Contributing

We welcome skill submissions from the community. See [CONTRIBUTING.md](./CONTRIBUTING.md) for the process and skill quality bar.

## License

MIT — see [LICENSE](./LICENSE). Individual skills retain their own attribution (workshop authors, framework creators, etc.) in their own READMEs.
