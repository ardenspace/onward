# Onward

Build with the next change in mind.

Agents ship an MVP fast. What costs you afterwards is everything the MVP quietly decided: a schema that now has to change in the backend, the frontend, and the migration at once; a color hardcoded in forty places because nobody found the tokens; a second modal because nobody found the first. Onward puts two short checks in front of that work so the expensive choices get made on purpose.

It is two skills, no runtime, no hooks, no review orchestration.

## Install

Claude Code:

```text
/plugin marketplace add ardenspace/onward
/plugin install onward@onward
```

Codex:

```sh
codex plugin marketplace add https://github.com/ardenspace/onward.git
codex plugin add onward@onward
```

Both hosts load the same two skills from `plugins/onward/`. They are explicitly invoked only; neither host applies them to ordinary requests on its own.

## Use

Establish the canon once per project, or when joining an existing one:

```text
/onward:spec          (Claude Code)
$spec                 (Codex)
```

Before each change, check it against the canon:

```text
/onward:plan Add a theme preview to the settings screen.
$plan Add a theme preview to the settings screen.
```

Then implement the way you normally do. Onward stops where implementation starts.

## What `spec` does

It reads the code first and asks only what the code cannot answer, one question at a time.

**The world.** What the product is in one picture, not a feature list ("a chat room where an assistant messages you"). Who it serves and in which moment. Where two invented future features would land inside that picture, which is the test of whether the picture holds. What stays out on purpose. Which priority wins when good options conflict.

**Expensive decisions.** Nine areas that are cheap to choose now and expensive to reverse later: data schema, data ownership, account model, multi-tenancy, public contracts, storage, platform, billing unit, shared foundations. Each is marked decided, not applicable, deferred with a trigger, or decide now. Only the last kind gets a question, in the form of a default plus one line of reversal cost:

> Settings will live on the device, since the product is personal and login is optional. If account sync is ever needed, that is one migration. OK?

**Shared foundations.** Where the tokens, shared components, and global helpers live, as pointers into the code. Planned-shared elements go in the shared place from first use; everything else starts local and is assessed at its second real use.

The result is one page, `docs/onward.md`, plus one line in `CLAUDE.md` or `AGENTS.md` telling the agent to read it before touching shared code.

## What `plan` does

Given a change request, it reads the canon and answers four things in chat:

1. **Class.** Too small to plan, fits the world, or conflicts with it. A conflict stops and asks whether the canon should change.
2. **Expensive decisions** this change newly introduces or alters. Using an existing decision is free; changing one gets the default-plus-reversal-cost question.
3. **Blast radius.** For each thing modified, the consumers that must change with it, found by search and named individually.
4. **Reuse and order.** Which shared elements to use, what to build local, and that a new shared foundation lands before its first consumer.

## What Onward does not do

- It does not implement, review, or verify. Use whatever your host already provides for that.
- It does not create registries, ledgers, status files, or process documents beyond the one canon page.
- It does not enforce anything during implementation. An agent that knows the tokens exist and hardcodes anyway is a review problem, not a planning problem.

## Design notes

Onward is the successor to [wellbegun](https://github.com/ardenspace/wellbegun), a five-skill pipeline with contracts, registries, reversal-cost grades, and independent verification rounds. It found real defects and cost days. The lesson was that the planning lenses carried the value and the execution machinery carried the cost, so Onward keeps the lenses and drops the machinery.

Kept from wellbegun: the one-way-door framing, the "deliberately unspecified" section, the request triage before planning, the non-goals question, the shared-element promotion rule, foundation-before-consumer ordering, a few blind-spot probes, and keeping superseded decisions visible. Dropped: S/M/L/XL grades, draft/approved status, decision ledgers, registry files, hooks, step contracts, gates, cycles.

Behavioral effect is unverified. The next step is to use it on a small real change and see whether the questions land.

## License

MIT
