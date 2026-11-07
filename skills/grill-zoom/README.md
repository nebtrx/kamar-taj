# grill-zoom

A design interview that finds the real problem first, then walks the solution one zoom level at a time, and always tells you where you are.

## Why I built this

I used Matt Pocock's [grill-me](https://github.com/mattpocock/skills) a lot. The idea is exactly right: before an agent builds anything, it should interview you until you both understand what's being built. But it never felt right to me, and it took me a while to say why.

**It felt rushed.** Not slow: rushed. Like someone taking decisions for me, in an endless stream. *And what about this? And what about this? So do this.* The questions came with recommended answers, so I ended up approving instead of deciding.

**I lost my sense of direction.** There was no way to know where I was or how much was left. When you want to finish and the questions keep coming, with no light at the end of the tunnel, you give up thinking and start saying yes.

**It mixed zoom levels.** Think of C4 diagrams. Sometimes you're working at the level of the whole system, sometimes inside one component, sometimes inside one function. grill-me makes no distinction. One question is about the overall architecture, the next is about a field inside one service, then it's back to the top. Going back and forth between levels is what wears you out.

**It went to decisions before the problem was clear.** What you first say you want is usually unstructured, sometimes inconsistent, and it rarely covers every corner. A good interviewer takes a few steps back first, until the main problem is on the table. Building anything means making trade-offs and leaving things out, so you need to know what you care about, what you don't, and what it costs to leave each thing out.

I wasn't the only one. The grill-me discussions are full of people with the same feeling ([#771](https://github.com/mattpocock/skills/discussions/771), [#831](https://github.com/mattpocock/skills/issues/831)).

I'm not reinventing the interview. Matt Pocock and Addy Osmani already got the mechanics right. What I changed is the *structure*, so it fits the way my brain works.

## What's different

| | grill-me | grill-zoom |
|---|---|---|
| Starts with | Decisions about your plan | The problem behind your plan: the driver, the gaps, what's in and out, and the consequences of each |
| Questions | Rounds of numbered questions | One at a time |
| Recommendations | On every question | None while finding the problem (they anchor you); in the solution phase, only where they help |
| Zoom | Mixed | One level at a time, overview first, then one component, then its internals |
| Where am I? | Not shown | Every message starts with your position in the tree and what's left |
| Cheap decisions | Asked like the expensive ones | Made as visible defaults you can veto |
| Across sessions | Lives in the chat | Saved to `design-tree.md`; pick up where you left off |
| Done when | Every branch visited | You confirm each layer, and the branches you care about are resolved |

## How a session goes

**1. Find the problem.** Open questions, no guesses. It steps back from what you asked for to what's actually driving it, names the gaps, and maps what you care about. Every few answers it tells you its current read and how confident it is. It ends with a problem brief you confirm explicitly.

**2. Walk the solution tree.** It starts with the overview (L1): the main parts and how they connect. Then it closes that layer, shows you the tree, and asks which part to zoom into next. It goes deep one branch at a time.

- **Climbing back up.** When something discovered deep down changes a higher level, it says so, goes up, makes the smallest change that fits, and comes back to exactly where you were.
- **Parked notes.** When something discovered concerns a different branch, it doesn't go there. It pins a 📌 note on that branch for later.

Example: designing on AWS, you're inside a Lambda (L3) and realise it needs a cache. Adding the cache changes the level above, so it climbs up. Filling the cache is a sibling component's job, so that becomes a 📌 note on the sibling.

**3. Always know where you are.** Every message opens with a line like:

```
📍 L2 › Ingestion · layer 2/4 settled · next zoom: Cache, API · 📌 2 · ⚠ 1
```

It never invents a total it can't know. Unknown depth stays unknown.

## Things you can say

| Say | What happens |
|---|---|
| `tree` | Show the whole tree |
| `zoom in <node>` / `zoom out` | Change level; your position is saved |
| `park this` | Turn the current topic into a 📌 note on the node it belongs to |
| `you're rushing` | It slows down: confirms the current premise and asks instead of defaulting |
| `you're dragging` | It speeds up: more defaults, fewer low-impact questions |
| `stop here` | It finishes or saves the current layer and tells you where to resume |

## See it run

The first real session was naming this repo: [examples/naming-this-repo.md](examples/naming-this-repo.md).

## Credits

Built on two MIT-licensed skills:

- **[grill-me / grilling](https://github.com/mattpocock/skills)** by Matt Pocock: the design tree, the frontier (only ask what's unblocked), "facts are the agent's job, decisions are yours", and nothing acted on until you confirm.
- **[interview-me](https://github.com/addyosmani/agent-skills)** by Addy Osmani: the confidence read, the "if you didn't have to justify it" probe, the restate with an explicit out-of-scope line, "sounds good" isn't a yes, and the stop test.

Licence notices for both are in [THIRD_PARTY_NOTICES.md](../../THIRD_PARTY_NOTICES.md).

## Status

v0.1. It has had one real run so far, and it will change as I use it more.
