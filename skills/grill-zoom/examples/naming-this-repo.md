# Design tree: skills repo name
Updated: 2026-11-07 · Phase: solution · Position: done

## Problem brief
- Driver: show that his skills are crafted, detailed tools that adapt to whoever wields them and keep them in control; not generic
- For whom: engineers interested in AI, "the right mindset"
- Why now: first skill (grill-zoom) is ready; the repo will hold all future ones
- Desired state: someone reads the name and thinks "a curator who hands me the tool that fits me"
- In scope: library/curator frame → signals knowledge and curation, not a toolbox
- In scope: pop-culture/geek flavour → memorable; risk: lost on people who don't get the reference
- Out of scope: collecting other people's skills → the name must not read as an aggregator or "awesome list"
- Doesn't matter: not discussed yet
- Must not: generic; aggregator vibe; must connect to skills (from spike)
- Gaps found: library vs tool message → resolved: library is the frame, and the curator hands out tools that adapt

## Tree
- ✅ L1 Name direction: which family of meaning the name draws from
  - ✅ Concept family: Marvel mystic/maker; shortlist kamar-taj, sanctum, forge
  - ✅ Skills link: pure name; description + topics carry the link
  - ✅ Candidates: kamar-taj
  - ✅ Form: slug kamar-taj; description and topics as defaults

## Decisions
| # | Node | Decision | Why | Reversible | ADR |
|---|------|----------|-----|------------|-----|
| 1 | Concept family | Marvel family; shortlist kamar-taj, sanctum, forge | Omar's picks from the spike | Yes | No |
| 2 | Skills link | Pure name; GitHub description + topics (agent-skills, claude-skills) carry the skills link | Elegant; reference already implies skills; install command says skills; rename redirects | Yes | No |
| 3 | Candidates | kamar-taj | Carries library + curator + learning skills; no dev-tool namesake (sanctum = Laravel Sanctum; forge = Laravel/Atlassian/Minecraft Forge) | Yes (rename redirects) | No |

## Defaults (cheap, vetoable)
| Node | Default | Why |
|------|---------|-----|
| Form | Slug `kamar-taj`, lowercase with hyphen | Matches the Marvel spelling and GitHub convention |
| Form | Display name "Kamar-Taj" in README | Proper spelling for humans |
| Form | Description: "A library of agent skills, each fitted to its wielder." | Carries the skills link (decision 2) |
| Form | Topics: agent-skills, claude-skills, claude-code, ai-agents | Discoverability |
| Form | Availability: check only under his own GitHub account; stop if it exists | Names are per-account on GitHub |
| Concept family | Drop q-branch, room-of-requirement, ollivanders, l-space, bespoke-shelf | Not picked in the spike |

## Spikes
- ✅ Concept family: spread of 8 names; picks were the three Marvel ones; concern raised: link to skills
