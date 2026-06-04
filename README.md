# Hawksfold Skills

A growing collection of [Claude Code](https://code.claude.com) skills, distributed as a
plugin marketplace.

## Skills

| Skill | What it does |
| --- | --- |
| [`challenge`](plugins/challenge/skills/challenge/SKILL.md) | Stress-test a plan or decision with a calibrated, balanced counter-view — surfaces the load-bearing assumption, triages objections by severity, and ends with the cheapest test to settle the question. |

## Install (recommended: via the marketplace)

From inside Claude Code:

```
/plugin marketplace add hawksfold/skills
/plugin install challenge@hawksfold-skills
```

The skill then triggers automatically when you ask Claude to *challenge*, *red-team*,
*pressure-test*, or *poke holes in* a plan — or invoke it directly from the `/` menu.

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
    └── challenge/
        ├── .claude-plugin/
        │   └── plugin.json        # plugin manifest
        └── skills/
            └── challenge/
                └── SKILL.md       # the skill itself
```

## Develop / test locally

Add this repo as a local marketplace, then install from it:

```
/plugin marketplace add /path/to/this/repo
/plugin install challenge@hawksfold-skills
```

Edits to a `SKILL.md` take effect within the session — no restart needed.
