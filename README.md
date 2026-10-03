# Onward

Build with the next change in mind.

Agents ship an MVP fast. What costs you afterwards is everything the MVP quietly decided: a schema that now has to change in the backend, the frontend, and the migration at once; a color hardcoded in forty places because nobody found the tokens; a second modal because nobody found the first. Onward puts two short checks in front of that work so the expensive choices get made on purpose, and one after it so drift from those choices gets caught.

It is three skills, no runtime, no hooks, no review orchestration.

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

Both hosts load the same three skills from `plugins/onward/`. They are explicitly invoked only; neither host applies them to ordinary requests on its own.

## Use

Establish the canon once per project, or when joining an existing one:

```text
/onward:spec          (Claude Code)
$onward:spec          (Codex)
```

Before each change, check it against the canon:

```text
/onward:plan Add a theme preview to the settings screen.
$onward:plan Add a theme preview to the settings screen.
```

Then implement the way you normally do. Onward does not run or steer implementation.

After implementing, check the result against the canon, ideally from a fresh session:

```text
/onward:check
$onward:check
```

## What `spec` does

It reads the code first and asks only what the code cannot answer, one question at a time.

**The world.** What the product is in one picture, not a feature list ("a chat room where an assistant messages you"). Who it serves and in which moment. Where two invented future features would land inside that picture, which is the test of whether the picture holds. What stays out on purpose. Which priority wins when good options conflict.

**Expensive decisions.** Nine areas that are cheap to choose now and expensive to reverse later: data ownership, account model, multi-tenancy, data schema, public contracts, storage, platform, billing unit, shared foundations. Each is marked decided, not applicable, deferred with a trigger, or decide now, and records its source: the user, a named document, or an unchecked agent default. Only decide-now items about intent get a question, phrased as a consequence the user can judge without knowing the stack, with a default and one line of reversal cost. Technical choices are not asked; they follow the framework's official docs or ecosystem default and are shown so the user can object:

> If you switch phones, your settings start over on the new one. Adding sync later is possible but means moving everyone's saved settings once. Is starting over on a new phone fine for now?

**Shared foundations.** Where the tokens, shared components, and global helpers live, as pointers into the code. Planned-shared elements go in the shared place from first use; everything else starts local and is assessed at its second real use.

**Enforced rules.** Lint and CI rules that encode a project convention, each with the problem it prevents in plain words, its source, and when an exception is allowed. A rule nobody can explain stays a warning, because an enforced rule nobody understands gets bypassed without anyone noticing.

The result is one page, `docs/onward.md`, plus one line in `CLAUDE.md` or `AGENTS.md` telling the agent to read it before touching shared code.

## What `plan` does

Given a change request, it reads the canon and answers five things in chat:

1. **Class.** Too small to plan, fits the world, or conflicts with it. A conflict stops and asks whether the canon should change.
2. **Expensive decisions** this change newly introduces or alters. Using an existing decision is free; changing one gets the default-plus-reversal-cost question, and the answer is written back into the canon with its source so the next plan does not ask again. A convention the change sets is proposed as a lint rule rather than a note, when its reason can be stated.
3. **Blast radius.** For each thing modified, the consumers that must change with it, found by search and named individually.
4. **Reuse and order.** Which shared elements to use, what to build local, and that a new shared foundation lands before its first consumer.
5. **Done when.** One to three observable completion criteria, fixed before implementation, that the implementer checks once at the end. Where the change touches stored data or other consumers, one of them says those still work.

## What `check` does

It looks at a finished change once, read-only, and reports in chat:

1. **Done when, rerun.** Each criterion is run again rather than taken from the implementer's report.
2. **Canon drift.** Recorded decisions altered, public contracts broken, shared foundations bypassed, enforced rules weakened, and new expensive decisions made without a canon row.
3. **For you.** What now works and what might break, in plain words, before the evidence.

It is not a bug hunt: the implementing agent already tests ordinary correctness and is usually right. It does not fix, loop, or keep rounds. Findings must cite a command or a file and line, and only breaking a criterion or the canon blocks.

## What Onward does not do

- It does not implement or run anything. `check` verifies one finished change against the canon and its criteria; general code review is your host's.
- It does not create registries, ledgers, status files, or process documents beyond the one canon page.
- It does not enforce anything during implementation. An agent that knows the tokens exist and hardcodes anyway is a review problem, not a planning problem.

## Design notes

Onward is the successor to [wellbegun](https://github.com/ardenspace/wellbegun), a five-skill pipeline with contracts, registries, reversal-cost grades, and independent verification rounds. It found real defects and cost days. The lesson was that the planning lenses carried the value and the execution machinery carried the cost, so Onward keeps the lenses and drops the machinery.

Kept from wellbegun: the one-way-door framing, the "deliberately unspecified" section, the request triage before planning, the non-goals question, the shared-element promotion rule, foundation-before-consumer ordering, a few blind-spot probes, and keeping superseded decisions visible. Dropped: S/M/L/XL grades, draft/approved status, decision ledgers, registry files, hooks, step contracts, gates, cycles.

Small synthetic experiments are recorded in [docs/2026-10-03-verification-experiments.md](docs/2026-10-03-verification-experiments.md): on small changes the implementing model already verifies itself well, so `check` looks for drift from the canon and false completion reports rather than hunting bugs. Behavior on real projects is still unverified; the next step is to use it on a few real changes.

## License

MIT
