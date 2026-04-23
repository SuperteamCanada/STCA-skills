# Contributing to STCA Skills

Thanks for wanting to contribute. This repo is Superteam Canada's curated collection of Claude skills for founders — we accept both new skills and improvements to existing ones.

## What makes a good skill

A skill earns its place in this repo if it meets **all four** of these:

1. **Encodes a specific workflow, framework, or playbook** — not just a one-paragraph prompt. A skill is a reusable chunk of expertise.
2. **Has a non-trivial "reference library"** — bundled examples, checklists, rubrics, or templates the agent can load as needed. Progressive disclosure: SKILL.md triggers, references load on demand.
3. **Triggers reliably** — the `description` frontmatter fires for realistic user queries, not just the exact skill name.
4. **Solves a problem a Canadian founder actually has** — pitching, fundraising, shipping, security, validation, go-to-market, etc.

Skills that are just "thin wrappers around Claude" (e.g. "answer questions like a lawyer") don't qualify — the value is in the *specific framework* the skill encodes.

## Directory structure

Each skill lives under `skills/<skill-name>/`:

```
skills/
└── your-skill-name/
    ├── SKILL.md              # required — frontmatter (name, description) + body
    ├── README.md             # required — describes the skill for humans browsing GitHub
    ├── references/           # optional — docs loaded on demand
    │   └── *.md
    └── assets/               # optional — templates, outlines, images
        └── *
```

Folder name **must match** the `name:` field in `SKILL.md` frontmatter.

## Skill quality bar

- `SKILL.md` under ~500 lines; heavy content goes in `references/`
- `description:` field tuned so the skill fires for realistic queries (use Anthropic's `skill-creator` optimization loop if you want rigor)
- No proprietary info in the skill — workshops, talks, public frameworks are fair game, private client data is not
- Credit the framework author / workshop presenter in the skill's README
- MIT-licensed by submitting (root LICENSE covers the repo)

## Submission process

1. **Fork** this repo (top-right of GitHub).
2. **Add your skill** under `skills/<skill-name>/`.
3. **Add a row** to the "Available skills" table in the root `README.md`.
4. **Open a Pull Request** against `main` with:
   - One-line description of what the skill does
   - A couple of realistic user queries that should trigger it
   - Who the framework / workshop author is (if derived from someone else's work)
5. A maintainer will review for the quality bar above and either merge, request changes, or explain why it doesn't fit.

## Improvements to existing skills

For fixes, additions, or description tuning on an existing skill: same fork + PR flow, but keep the PR scoped to one skill. Describe what changed and why in the PR body.

## Questions?

Open a GitHub issue or ping Superteam Canada on Telegram.
