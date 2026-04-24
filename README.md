# STCA Skills

A curated collection of agentic skills for founders — maintained by [Superteam Canada](https://superteam.ca/).

Each skill encodes a specific workflow, framework, or playbook so any founder with a Claude-powered assistant (Claude Code, Claude.ai, Copilot CLI, Gemini CLI, etc.) can tap into expert-level guidance on demand.

## Available skills

### Pitch & Design

| Skill | What it does |
|---|---|
| [winning-pitch-deck](./skills/winning-pitch-deck/) | Craft or review a 3-minute founder pitch deck and speaker script using the DePitch framework. Built for Colosseum Frontier. |
| [ui-ux-pro-max](./skills/ui-ux-pro-max/) | Design intelligence with 50+ styles, 161 color palettes, 57 font pairings, and 99 UX guidelines. Priority-ranked from accessibility to charts. |

### Solana Ecosystem

| Skill | What it does |
|---|---|
| [solana-agent](./skills/solana-agent/) | Scaffold a Solana AI agent using SendAI's Agent Kit v2 and the Solana Developer MCP. Plugin selection, embedded wallets, devnet deployment. |
| [solana-blinks](./skills/solana-blinks/) | Build Solana Actions and Blinks — shareable transaction URLs that render natively inside X (Twitter), Phantom, and other clients. |
| [solana-sdp](./skills/solana-sdp/) | Build on the Solana Developer Platform for enterprise payments, stablecoin issuance, and trading. Mastercard/Worldpay/Western Union-grade infrastructure. |
| [solana-compress](./skills/solana-compress/) | Launch ZK compressed tokens using Light Protocol. 99% cost reduction for airdrops, loyalty programs, gaming assets, and mass distribution. |
| [solana-hackathon](./skills/solana-hackathon/) | Bootstrap a hackathon project with the right 2026 primitives. Full scaffold, submission package, and time management. Pairs with all other skills. |

### How they work together

```
Idea → solana-hackathon (scaffold + primitives)
         ├── solana-agent (if AI + crypto)
         ├── solana-blinks (if social distribution)
         ├── solana-sdp (if payments/fintech)
         ├── solana-compress (if mass token ops)
         ├── ui-ux-pro-max (design the frontend)
         └── winning-pitch-deck (pitch video)
```

## Install a skill

### Claude Code

Clone only the skill you want:

```bash
git clone --depth 1 --filter=blob:none --sparse \
  https://github.com/SuperteamCanada/STCA-skills.git \
  ~/.claude/skills/_stca-tmp
cd ~/.claude/skills/_stca-tmp
git sparse-checkout set skills/<skill-name>
mv skills/<skill-name> ~/.claude/skills/<skill-name>
cd .. && rm -rf _stca-tmp
```

Or clone the whole collection and symlink what you want:

```bash
git clone https://github.com/SuperteamCanada/STCA-skills.git ~/code/STCA-skills
ln -s ~/code/STCA-skills/skills/<skill-name> ~/.claude/skills/<skill-name>
```

Restart Claude Code. The skill is available in all your sessions.

### Install all skills at once

```bash
git clone https://github.com/SuperteamCanada/STCA-skills.git ~/code/STCA-skills
for skill in ~/code/STCA-skills/skills/*/; do
  ln -sf "$skill" ~/.claude/skills/$(basename "$skill")
done
```

## Contributing

We welcome skill submissions from the community. See [CONTRIBUTING.md](./CONTRIBUTING.md) for the process and skill quality bar.

## License

MIT — see [LICENSE](./LICENSE). Individual skills retain their own attribution (workshop authors, framework creators, etc.) in their own READMEs.
