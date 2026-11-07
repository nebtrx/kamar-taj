# Kamar-Taj

> A library of agent skills, each fitted to its wielder.

In the Marvel films, Kamar-Taj is where Stephen Strange goes to learn. Wong keeps the library. The books don't make you a sorcerer; they meet you where you are and give you the skill you need.

That's the idea here. These are skills for AI agents (Claude Code, the Claude apps, anything that reads a `SKILL.md`). I write them myself, and I write them in detail. They aren't generic. Each one is built around how I actually think and work, and written so you can bend it to how *you* think and work.

What that means in practice:

- **Detailed and opinionated.** Every skill has a clear point of view about how the work should go.
- **You stay in control.** The skills structure the work; they don't take decisions away from you.
- **The why is written down.** Each skill has its own README: what bothered me, what I changed, and why.
- **Credit where it's due.** When a skill builds on someone else's work, its README says so, and the licence notices are in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

## The library

| Skill | What it does | Run it with |
|---|---|---|
| [grill-zoom](skills/grill-zoom/) | A design interview that finds the real problem first, then walks the solution one zoom level at a time and always shows where you are | `/grill-zoom` |

## Install

**Claude Code, with the skills CLI**

```bash
npx skills add nebtrx/kamar-taj --skill grill-zoom
```

**Claude Code, by hand**

```bash
git clone https://github.com/nebtrx/kamar-taj
mkdir -p ~/.claude/skills
cp -r kamar-taj/skills/grill-zoom ~/.claude/skills/
```

**Claude apps (web, desktop)**

Zip the `skills/grill-zoom` folder, then go to **Settings → Capabilities → Skills → Upload skill**.

## Make it yours

Skills are plain markdown. Fork the repo, change the parts that don't fit how you think, and keep the rest. That's the whole point.

## Licence

[MIT](LICENSE). Third-party notices are in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

Kamar-Taj is a Marvel name, used here as a fan reference. This project isn't affiliated with Marvel or Disney.
