# Hawksfold Skills

A growing collection of [Claude Code](https://code.claude.com) skills, distributed as a
plugin marketplace.

## Skills

| Skill | What it does |
| --- | --- |
| [`challenge`](plugins/challenge/skills/challenge/SKILL.md) | Stress-test a plan or decision with a calibrated, balanced counter-view — surfaces the load-bearing assumption, triages objections by severity, and ends with the cheapest test to settle the question. |
| [`red-teaming-review`](plugins/red-teaming-review/skills/red-teaming-review/SKILL.md) | Two-round adversarial review of any output — attacks from six lenses with severity/confidence triage, persists deferrals to `docs/technical-debt.md`, and synthesizes the strongest version for you to approve. Heavier than `challenge`; for deliverables you're about to act on. |

## Install (recommended: via the marketplace)

From inside Claude Code:

```
/plugin marketplace add hawksfold/skills
/plugin install challenge@hawksfold-skills
/plugin install red-teaming-review@hawksfold-skills
```

`challenge` triggers automatically when you ask Claude to *challenge*, *red-team*,
*pressure-test*, or *poke holes in* a plan. `red-teaming-review` is the heavier,
two-round pass — it triggers on *attack my plan* / *find weaknesses* or after a
significant deliverable you're about to act on. Either can be invoked directly from
the `/` menu.

## Install (manual, no plugin)

Copy a skill's directory into your personal or project skills folder — Claude Code
auto-discovers `SKILL.md` files there, no manifest needed:

```
# Personal (all projects)
cp -r plugins/challenge/skills/challenge ~/.claude/skills/challenge

# Project-only
cp -r plugins/challenge/skills/challenge .claude/skills/challenge
```

## Layout

```
.
├── .claude-plugin/
│   └── marketplace.json          # lists the plugins in this repo
└── plugins/
    ├── challenge/
    │   ├── .claude-plugin/
    │   │   └── plugin.json        # plugin manifest
    │   └── skills/
    │       └── challenge/
    │           └── SKILL.md       # the skill itself
    └── red-teaming-review/
        ├── .claude-plugin/
        │   └── plugin.json
        └── skills/
            └── red-teaming-review/
                └── SKILL.md
```

## Develop / test locally

Add this repo as a local marketplace, then install from it:

```
/plugin marketplace add /path/to/this/repo
/plugin install challenge@hawksfold-skills
```

Edits to a `SKILL.md` take effect within the session — no restart needed.
