---
name: grill-zoom
description: Problem-first design interview that works one zoom level at a time. First finds the real problem behind what the user says they want (main driver, desired state, what's in, what's out, and the consequences of each), then walks the solution as a tree, overview first and then one component at a time, always showing where you are and what's left. Use when the user says "grill me", "interview me", "zoom grill", wants to think through a design, architecture, feature or plan before building it, or wants to resume a design tree from an earlier session.
disable-model-invocation: true
---

# Grill Zoom

Derived from Matt Pocock's `grill-me` / `grilling` (MIT) and Addy Osmani's `interview-me` (MIT). Their mechanics are kept: the design tree, the frontier, facts-are-your-job, the explicit-yes gate, the restate, the stop test. The structure around them is replaced.

## Why this skill exists

Plain grilling fails in a specific way: it feels rushed. Decisions are made for you, one "what about this?" follows another with no end in sight, questions jump between zoom levels, and nothing shows how much is left. The user loses direction and starts approving instead of deciding.

This skill fixes the structure, not the pace:

- **Problem before solution.** Time spent understanding the problem is welcome.
- **One zoom level at a time.** Overview first, then one component, then its internals.
- **Visible position.** Every message says where you are in the tree and what's ahead.
- **The user stays in control.** They pick where to zoom, can stop at any layer boundary, and can override the pace.

Steer between two failure modes:

| | Rushed | Slow |
|---|---|---|
| What it is | Moving past something not yet confirmed | Asking things whose answer wouldn't change the next step |
| Check | Before advancing: "Is everything this step builds on confirmed?" | Before asking: "Would the answer change what I do next, and is it something only the user knows?" |
| If it fails | Stop and confirm the premise | Look it up, or decide it as a visible default |

## Ground rules (every phase)

1. **One question per message, then wait.** Batches get skimmed and rubber-stamped; a single question gets thought about.
2. **Facts are your job, decisions are the user's.** Before asking, look in the codebase, docs, ADRs, CONTEXT.md, and design-tree.md. Use a sub-agent for lookups when one is available. Ask the user only about intent, priorities, constraints and trade-offs.
3. **Never silently reword a question you already asked.** If new facts change it, say what changed in one line.
4. **Keep turns short:** the position header, at most about 100 words, and one question. Turns should read well aloud, so the skill also works in voice mode, where conversation feels less rushed than reading walls of text.
5. **Start every message with the position header** (see "Where are we").
6. **Don't implement.** The output is a confirmed problem and a design, not code.

## Phase 0: Orient (one or two turns)

- **If design-tree.md exists:** read it, show the tree, say where the last session stopped, and list the parked notes on the next node. Ask whether to continue there.
- **Otherwise:** gather the facts you can reach first. Then capture the ask in the user's own words, in one line.

## Phase 1: Find the problem

Goal: a **Problem Brief** the user explicitly confirms. No solutions yet. If the user jumps to one, park it as a candidate for Phase 2 ("noted for the solution phase") and continue.

What the user first says they want is usually unstructured and inconsistent, and it rarely covers every corner. Work through these in order, moving on when each one is clear rather than when it has been asked about:

1. **The ask:** what they want to do, in their words.
2. **Step back to the driver.** Ask why, what breaks today, for whom, why now, and what happens if nothing is done. Ladder on each answer ("and why does that matter?") until the answer stops changing or reaches a goal the user cares about for its own sake. Stop when the next "why" leaves what the user can act on.
3. **Gaps.** Compare the ask with the driver. Look for parts of the ask that don't serve the driver, parts of the driver the ask doesn't cover, and contradictions. Name them one at a time.
4. **Desired state.** What is true when this is solved, and how would they know? Prefer observable signals.
5. **Care map.** Go through the aspects that could matter: users, quality attributes, cost, time, operability, compliance, team. Mark each as in, out, or doesn't matter, and ask for the consequence of that choice. Building something means choosing what to leave out, so the consequences of leaving things out must be stated explicitly.
6. **Non-desired states.** What must not happen: failure modes, and lines not to cross.

**How to ask in Phase 1**

- Ask open questions. Don't attach a guess or a recommendation: anchoring here locks in the wrong frame, and that is exactly what made grilling feel rushed.
- If the user asks "what do you think?", answer with your reasoning.
- Offer options only when the space is genuinely narrow and the user is stuck.
- If the user can only judge an item once they see examples ("I'll know it when I see it"), don't push. Mark it 🧪 in the brief and make it the first spike of Phase 2: a deliberately varied set of probes whose rejections become the answer.

**Current read.** Every two or three answers, or whenever your understanding shifts, give one line:

`Current read: <one sentence> · confidence ~N% · missing: <what>`

This is Phase 1's progress indicator and the user's chance to correct you early.

**Probes**

- **Convention or sophistication answers** ("scalable", "clean", "the standard way", "I should probably…"). Ask: "If you didn't have to justify this to anyone, what would you actually want?"
- **"Whatever you think" or "sounds good"** doesn't settle a Phase 1 item. Re-ask it as a choice between two concrete readings.

**Stop test.** You can predict the user's reaction to the next three questions you would ask. If three rounds go by without your confidence rising, say so, step back, and reframe instead of grinding on.

**Problem Brief.** Restate, then get an explicit yes:

```
Problem brief
- Driver:         <main problem or goal, one line>
- For whom:       <who has it>
- Why now:        <what changed>
- Desired state:  <what's true when solved; how we'll know>
- In scope:       <aspect> → consequence: <...>
- Out of scope:   <aspect> → consequence: <...>
- Doesn't matter: <aspects we won't optimise>
- Must not:       <non-desired states; 🧪 for any to be found from examples>
- Gaps found:     <gaps in the original ask, and how each was resolved>
Yes / no / refine?
```

After the yes, announce the switch ("Problem confirmed. Moving to the solution, level 1: overview.") and write the brief to design-tree.md.

## Phase 2: Walk the solution tree

### Zoom levels

Levels are relative, like zooming in C4:

- **L1, overview:** the main parts of the solution and how they connect.
- **L2:** the inside of one part: its components and their contracts.
- **L3 and below:** the inside of one component: data, conditions, failure handling.

Each node's children sit one level below it.

**Level discipline.** At a given layer, settle only what belongs at that level:

- which children exist
- what each child is responsible for
- how the children connect
- decisions that change the shape of the layer

Anything finer belongs to a child. Record it as an open item on that child and move on. Mixing levels is what causes the back-and-forth, so when a question belongs elsewhere, say where it belongs and defer it.

### The unit of work: one layer

A layer is one node's children, together with their responsibilities, their connections, and the decisions made at that level.

1. **Open the layer.** Ask how the user sees the parts. If they would rather react to a sketch, propose one, label it as a draft, and tie each part back to the Problem Brief.
2. **Work through the layer's frontier one question at a time.** The frontier is the set of decisions at this layer whose prerequisites are already settled. A question that depends on an open answer waits.
3. **Close the layer.** Play back the layer's tree, its decisions and its defaults in a few lines. Get an explicit confirmation, then write it to design-tree.md.
4. **Announce the next zoom.** List the children in a proposed order, riskiest or most uncertain first (or dependency order when one blocks another), with one line of reason. The user picks. Always add: "Or stop here; the tree is saved."

**Session guarantee.** A session completes at least one full layer, and layer boundaries are the natural stopping points. If the user has to stop mid-layer, save the layer as in progress, record what's settled and what's open, and say so plainly.

### Traversal

**Default: depth-first.** After closing a node's layer, zoom into the child the user picked and continue down that branch until it's resolved to the depth needed. Then climb back and take the next sibling.

**"Resolved to the depth needed"** means the branch's remaining decisions are cheap to reverse or plain implementation detail, so zooming further wouldn't change cost or risk. Say so when you stop descending. The user can override with "go deeper".

The user can redirect at any time ("zoom into X", "zoom out", "switch to Y"). Honour it and record the current position so you can return.

### Climbing back up

When a discovery at depth changes a higher layer (a new node is needed, a responsibility moves, an interface changes):

1. **Stop and say it:** "This changes L1: <what>. Go up and update it?"
2. **On yes,** go up and make the smallest change that fits. Confirm it in one line, and mark the affected nodes ⚠ with a note of what to re-check.
3. **Return to exactly where you were:** "Back to L3 › Order Lambda."

While you're up there, don't redesign the upper layer: change only what the discovery forced.

### Parked notes

When a discovery concerns another branch (a sibling or a cousin), don't go there. Attach a 📌 note to that node saying what it needs, why, and where the note came from. When you later arrive at a node, its parked notes are the first items on the agenda.

**Example.** At L3, inside the order Lambda, you find it needs a cache.

- Adding the cache changes the parent layer, so it is a climb-up.
- Filling the cache belongs to a sibling component, so it gets a parked note: "📌 from Order Lambda: must populate the cache it reads; freshness requirement still open."

### How to ask in Phase 2

- **One question.** Where the space is narrow, give options with their trade-offs, then your recommendation with a one-line reason; a "yes" accepts it. Recommendations are fine here because the problem is now shared, so they no longer anchor the frame.
- **Spend questions in proportion to the cost of being wrong:**
  - **Expensive or hard to reverse:** ask, one at a time, with the trade-offs explicit. Flag a decision as ADR-worthy when it is hard to reverse, would surprise a future reader without context, and was a real trade-off.
  - **Cheap and reversible:** don't ask. Decide, and list these as defaults at layer close so the user can veto any of them. Keep it to about five per layer. If there are more, some of them aren't really cheap, so ask about those.
- **Tie back to the brief.** Before adding a node or capability, check that it serves the driver or the desired state. If it conflicts with Out of scope, say so and ask.
- **Questions that are hard to settle in the abstract** (ones that are easier to judge once you can see it): propose a throwaway spike or prototype, record it as a 🧪 node, and move on.
- **"Your call":** on a cheap item, decide it and log it as a default. On an expensive one, offer two concrete options and ask the user to pick.

## Where are we: progress

**Position header,** the first line of every message:

- Phase 1: `📍 Problem › driver · read ~60% · next: desired state`
- Phase 2: `📍 L2 › Ingestion · layer 2/4 settled · next zoom: Cache, API · 📌 2 · ⚠ 1`

**The full tree** appears at every layer close, on resume, and whenever the user says "tree".

**Don't invent precision.** Count only what's known: open items at this layer, and known nodes not yet zoomed into. Unknown depth stays unknown; write "depth unknown below" instead of guessing a total.

Tree markers: `✅` settled · `◐` in progress · `▶` current · `○` not started · `⚠` re-check after a climb-up · `📌` parked note · `🧪` spike

```
▶ L1 Overview: event-driven order intake
  ✅ Intake API: accepts and validates orders
  ◐ Order Lambda: prices and persists orders
     open: retry policy on pricing timeout
  ○ Cache filler: keeps pricing cache warm
     📌 from Order Lambda: must populate the cache it reads; freshness TBD
  ⚠ Notifications: re-check, order status now comes from DynamoDB stream
```

## design-tree.md: persistence across sessions

If you can write files, keep `design-tree.md` in the project root, or wherever the user says. Update it at every layer close, every climb-up, every new parked note, and every stop. It is the source of truth when resuming.

Keep the format exactly as below so a tool, such as a side panel, can render it.

```markdown
# Design tree: <topic>
Updated: <YYYY-MM-DD> · Phase: <problem|solution> · Position: <path, e.g. L1 > Order Lambda>

## Problem brief
<the confirmed brief>

## Tree
- ✅ L1 Overview: <one line>
  - ✅ <Node>: <responsibility>
  - ◐ <Node>: <responsibility>
    - open: <open item>
    - 📌 from <node>: <note>
  - ○ <Node>: <responsibility>
  - ⚠ <Node>: <responsibility> · re-check: <what changed>

## Decisions
| # | Node | Decision | Why | Reversible | ADR |
|---|------|----------|-----|------------|-----|

## Defaults (cheap, vetoable)
| Node | Default | Why |
|------|---------|-----|

## Spikes
- 🧪 <node>: <what the prototype must answer>
```

The tree uses nested bullets. Each node line starts with exactly one status marker and ends with its responsibility. Notes (`open:`, `📌 from`) sit as children of the node they belong to.

Without file access, keep the same structure in the conversation. Print it on "tree" and at every stop, so the user can paste it into the next session.

## Controls the user can say

| Say | Effect |
|---|---|
| `tree` | Print the full tree |
| `zoom in <node>` / `zoom out` | Change level; the current position is recorded |
| `park this` | Turn the current topic into a 📌 note on the node it belongs to |
| `you're rushing` | Confirm the current premise, play back the layer, ask about items instead of defaulting them |
| `you're dragging` | More defaults, skip low-impact questions, lead with recommendations |
| `stop here` | Finish or save the current layer, write the file, and say where to resume |
| `your call` | On cheap items, decide and log a default |

## Finishing

You're done when either of these holds:

- Every branch the user cares about is resolved to the depth needed, every 📌 note has been handled, and no ⚠ remains.
- The user stops.

The final message contains:

- the driver, in one line
- the tree
- decisions and defaults
- open items, outstanding parked notes, and spikes

Don't start implementing. Offer the next step: a spec, tickets, or ADRs for the flagged decisions.
