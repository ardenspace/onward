---
name: spec
description: Establish a project's canon once - the product in one picture, the decisions that are expensive to reverse, and where shared foundations live - as one short docs/onward.md. Use only when the user explicitly invokes this skill; never apply it automatically.
disable-model-invocation: true
---

# onward:spec - establish the canon

Write down, once per project, the things later changes must not contradict:
what this product is in one picture, the decisions that are expensive to
reverse, and where the shared foundations live. The output is one file,
`docs/onward.md`, short enough to read before every change.

Everything else stays deliberately unspecified. A canon that is dense
everywhere is as useless as one that is empty. Dense at the expensive
choices, silent at the cheap ones.

## Rules that apply throughout

- **Code answers first.** Never ask what the repository already shows. Read
  before asking.
- **One question at a time.** Ask the next question only after the previous
  one is answered. Bundle several confirmations into one message only when
  they are independent of each other.
- **Ask only what the user can answer.** The user may not know this stack.
  Every open decision is one of two kinds:
  - *Intent* - what matters, who uses it, what must never be lost, what may
    break. Ask the user; they are the expert here. Phrase it as a
    consequence in their world, with no jargon: not "device or account
    storage?" but "if you switch phones, is it fine that settings reset?"
  - *Expertise* - which pattern, library, or structure. Do not ask. Follow
    the framework's official documentation or the ecosystem default, name
    that source, prefer the more reversible option when sources disagree,
    and show the choice in one line so the user can object. If no source
    exists, record it as `agent default`, which means nobody has checked it.
- **Default plus reversal cost.** When an intent question is open, propose
  one default grounded in the world picture, state in one clause what
  reversing it later would cost in the user's terms, and ask for
  confirmation. Lay out alternatives only when the user declines the default
  or the alternatives are genuinely close.
- **Never answer for the user.** If an intent question goes unanswered - the
  session ends, the user changes subject - record it as `open` in the file
  and re-ask next time. An unanswered intent question never becomes an agent
  decision.
- **Do not re-ask settled decisions.** A decision already in `docs/onward.md`
  or stated in existing prose (README, PRD, ADR, design notes) is recorded,
  not reopened. A decision only the code shows is confirmed once, as Act 0
  describes, then treated as settled. Reopen only when the user asks.
- **Do not turn spec into a refactor.** If an expensive area is already
  spread through the code in a way that conflicts with a good choice, record
  the current state and note that changing it is a separate task. Do not fix
  it here and do not plan the fix here.

## Act 0: Read before asking

If `docs/onward.md` exists, read it and resume: skip every section that is
already filled, re-ask anything marked `open`, and continue from the first
gap. Do not start over. Before skipping the shared foundations table, check
that each location it points to still exists; fix a moved path from the code
and report a missing one as `open` instead of guessing a replacement.

Otherwise, if the project has code, find and read what answers the questions
below:

- Theme or token files (`tokens.*`, `theme.*`, `tailwind.config.*`, design
  system folders) - shared foundations.
- Shared component folders (`components/shared`, `ui/`, `common/`) and their
  most-used exports - shared foundations.
- Global utilities, helpers, state stores, i18n setup - shared foundations.
- Schema, models, migrations, ORM config - data schema, storage, ownership.
- Auth setup, tenant or workspace concepts, billing code - account model,
  multi-tenancy, billing unit.
- Public API routes, exported file formats, URL structure - public contracts.
- Lint, type-check, and CI configuration (`eslint.config.*`, `biome.json`,
  `analysis_options.yaml`, `ruff.toml`, workflow files) - enforced rules.
- `CLAUDE.md`, `AGENTS.md`, `README.md`, existing ADRs or design notes -
  decisions already made in prose.

Note what you found. A finding stated in prose (README, PRD, ADR, design
notes) becomes a `decided` row or a shared foundation entry without a
question. A finding only the code shows may have been decided by accident:
in Act 2, show all of those in one message as proposed `decided` rows and ask
which, if any, were not deliberate. A row the user flags becomes `decide now`.
A new project with no code skips this act.

## Act 1: The world

Five lenses, in this order, one question each. For an existing project,
propose an answer from what the code and UI show, then ask whether it is
right; do not make the user describe what you can see.

### 1. The world in one picture

Ask what this product is in one frame, not a feature list. Examples of the
kind of answer wanted: "a chat room where an assistant messages you", "one
notebook", "a control tower". The frame is what lets unrelated-looking
features belong together and tells future features where they land.

If the product has several features that do not obviously belong together,
say so and look for the frame with the user. If no frame emerges, write
"no single picture yet" rather than forcing one; do not invent a frame the
user has not recognized.

Probe when the answer looks thin: "What does the very first screen show
before any data exists?" The empty state exposes what the product thinks it
is.

### 2. Who, and the moment that matters

Ask whose experience matters most and which moment in their day this product
serves. One person, one moment: "me, five minutes before leaving work, needing
a dinner place decided". This is what later priorities are judged against.

### 3. How new things enter this world

Invent two plausible future features this product might grow - things the
user has not mentioned - and ask where each would appear inside the frame.
"If scheduling arrives, how does it show up in the chat room?" A natural
answer means the frame holds. A stuck answer means the frame is too narrow
or that feature is out of scope; record whichever the user decides.

Keep the answers. They are the examples `onward:plan` uses to judge whether
a new request fits the world.

### 4. What stays out

Ask what is deliberately excluded even though it might look like it belongs.
The section is not closed until it contains at least one thing the user
admits was tempting. This list is what lets a later plan say "this was
excluded on purpose" instead of silently expanding the product.

### 5. When good options conflict, and feel

Ask which priority wins when two good options collide. Two or three rules
are enough: "fast answer over exact answer, except when money is involved".
Then ask for the feel in one reference product or three adjectives. Skip the
feel question if lens 1 already produced it.

## Act 2: Expensive decisions

Walk this list after Act 1, because the world picture is what makes a default
defensible. The rows are in dependency order: ownership and tenancy shape the
schema, the schema shapes contracts. For each area, assign one status before
asking anything:

| area | why it is expensive to change later |
|---|---|
| data ownership | device vs account vs team; changing it creates sync and permission models that did not exist |
| account and auth model | login required or optional, guests; every screen and API assumes the answer |
| multi-tenancy | personal vs team vs organization; added later, every query grows a tenant clause |
| data schema | backend, frontend types, migrations and stored data all follow |
| public contracts | API shape, URL structure, event formats, files users keep; once something outside depends on it, it is frozen |
| storage location and kind | local, server, cloud; SQL, document, file; moving means a migration |
| platform and framework | web, mobile, desktop, runtime; effectively a rewrite |
| billing unit | seat, usage, project; seeps into the data model and every screen |
| shared foundations | theme tokens, component library, i18n, state management; replacement cost grows with each consumer |

Statuses:

- **decided** - existing notes answer it: record the choice and ask nothing.
  If only the code answers it, it goes into the single Act 0 confirmation
  message below before it is recorded as `decided`.
- **n/a** - does not apply to this product (a CLI has no multi-tenancy).
  Record with a one-clause reason.
- **decide now** - the MVP's code will bake this in soon, so leaving it open
  means it gets decided by accident. Ask.
- **deferred** - can stay open without spreading into code yet. Record with a
  concrete trigger for revisiting: "before the first paid user", "at the
  first multi-device request".
- **open** - a `decide now` item that was asked but not answered. Record it
  with `-` as the choice and repeat the question under `## Open`; it is
  re-asked on the next run and never filled in by the agent.

First send the Act 0 confirmation of code-only `decided` rows, if any; a
row the user flags becomes `decide now`. Split the `decide now` items into
intent and expertise. Expertise items are settled from their source and
shown together in one message, not asked. Then ask the intent items one per
message, in table order so the ones others depend on come first, using
default plus reversal cost:

> If you switch phones, your settings start over on the new one. Adding
> sync later is possible but means moving everyone's saved settings once.
> Is starting over on a new phone fine for now?

After an answer, derive the next default from it. Put two items in the same
message only when neither default depends on the other's answer. A new
project may have five or six `decide now` items; that is normal and is the
point of this skill.

Probes to use inside the relevant areas when the answer looks thin:

- ownership and storage: "A user disappears for two weeks and comes back.
  What is still there?"
- public contracts: "If anything is shared or exported, what does the
  recipient - no account, no context - actually see?"

If the user wants to change an area that is already `decided` and spread
through code, record the current state and mark the change as a separate
task. Do not resolve it here.

## Act 3: Write it down

1. **Shared foundations table.** Combine what Act 0 found with what Act 2
   decided under `shared foundations`. One row per kind, pointing at the
   location; the code holds the values. Apply the promotion rule and state it
   in the file: elements planned as shared live in the shared location from
   first use; anything else starts local and is assessed for promotion at its
   second real use. Similar appearance alone does not justify an abstraction.
2. **Deliberately unspecified.** Propose the list of things the file will not
   constrain - internal state shapes, helper structure, non-shared endpoint
   details, copy tone - and let the user glance at it. This tells future
   implementers where they are free.
3. **Write `docs/onward.md`** from the template below. Keep it to one page.
   If the project keeps design notes elsewhere and the user prefers that
   location, use it instead and say so.
4. **Enforced rules.** For each lint or CI rule that encodes a project
   convention (not a stock preset), record three things in the user's
   words: the problem it prevents, its source, and when an exception is
   allowed. A rule whose problem nobody can state stays a warning, not an
   error, until someone can; an enforced rule nobody understands gets
   bypassed, and nobody can tell whether the bypass was wrong. Exceptions
   use a disable comment with a stated reason; prefer configuring the tool
   to reject reasonless or unused disables.
5. **Point the host at it.** Add one line to the project instruction file the
   current host reads - `CLAUDE.md` in Claude Code, `AGENTS.md` in Codex - and
   to both only if the project already has both:
   `Read docs/onward.md before changing shared code, data structures, or public contracts.`
   Do not create the other host's file.
6. **Report** what was decided and by whom (user, a named source, or agent
   default), what is deferred with its trigger, and what is still `open`.

## Output template

```markdown
# <project> canon

## The world in one picture
<one sentence frame; or "no single picture yet">

## Who, and the moment that matters
<one person, one moment>

## How new things enter this world
- <future feature> -> <where it appears in the frame>
- <future feature> -> <where it appears in the frame>

## What stays out
- <excluded thing> (tempting because <reason>)

## When good options conflict
- <priority> over <priority>, except <condition>

## Feel
<one reference product or three adjectives>

## Expensive decisions
| area | status | choice | source | reversal cost if wrong | revisit when |
|---|---|---|---|---|---|
| data ownership | decided | device-local | user | one migration to account sync | first multi-device request |
| state management | decided | local state + context | docs: react.dev "Choosing the State Structure" | refactor per screen | first cross-screen state |
| billing unit | deferred | - | - | - | before first paid user |
| multi-tenancy | n/a | personal product | - | - | - |

`source` is `user`, `docs: <where>`, `code` (confirmed in Act 0), or
`agent default`. An `agent default` row has not been checked by anyone.
| data schema | open | - | - | next spec run |

When a decision changes, keep the row and add `was: <old> (<date>, <reason>)`
in the choice cell. Do not delete history.

## Shared foundations
| kind | location | use when |
|---|---|---|
| theme tokens | src/styles/tokens.css | any color, spacing, or font |
| modal | src/components/Modal.tsx | any dialog |

Planned shared elements live here from first use. Everything else starts
local and is assessed for promotion at its second real use.

## Enforced rules
| rule | where | prevents (in plain words) | source | exception allowed when |
|---|---|---|---|---|
| no setState inside useEffect | eslint.config.js, no-restricted-syntax | screens flashing wrong values and extra re-renders | docs: react.dev "You Might Not Need an Effect" | syncing with something outside React, with a reason comment |

## Deliberately unspecified
- <area left to the implementer>

## Open
- <question asked but not answered>
```
