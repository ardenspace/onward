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
- **Default plus reversal cost.** When a decision is open, propose one default
  grounded in the world picture, state in one clause what reversing it later
  would cost, and ask for confirmation. Lay out alternatives only when the user
  declines the default or the alternatives are genuinely close.
- **Never answer for the user.** If a question goes unanswered - the session
  ends, the user changes subject - record it as `open` in the file and re-ask
  next time. An unanswered question never becomes an agent decision.
- **Do not re-ask settled decisions.** A decision already in `docs/onward.md`
  or clearly expressed by code is recorded, not reopened. Reopen only when the
  user asks.
- **Do not turn spec into a refactor.** If an expensive area is already
  spread through the code in a way that conflicts with a good choice, record
  the current state and note that changing it is a separate task. Do not fix
  it here and do not plan the fix here.

## Act 0: Read before asking

If `docs/onward.md` exists, read it and resume: skip every section that is
already filled, re-ask anything marked `open`, and continue from the first
gap. Do not start over.

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
- `CLAUDE.md`, `AGENTS.md`, `README.md`, existing ADRs or design notes -
  decisions already made in prose.

Note what you found. Each finding becomes a `decided` row or a shared
foundation entry in Act 3 without a question. A new project with no code
skips this act.

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
defensible. For each area, assign one status before asking anything:

| area | why it is expensive to change later |
|---|---|
| data schema | backend, frontend types, migrations and stored data all follow |
| data ownership | device vs account vs team; changing it creates sync and permission models that did not exist |
| account and auth model | login required or optional, guests; every screen and API assumes the answer |
| multi-tenancy | personal vs team vs organization; added later, every query grows a tenant clause |
| public contracts | API shape, URL structure, event formats, files users keep; once something outside depends on it, it is frozen |
| storage location and kind | local, server, cloud; SQL, document, file; moving means a migration |
| platform and framework | web, mobile, desktop, runtime; effectively a rewrite |
| billing unit | seat, usage, project; seeps into the data model and every screen |
| shared foundations | theme tokens, component library, i18n, state management; replacement cost grows with each consumer |

Statuses:

- **decided** - the code or existing notes answer it. Record the choice; ask
  nothing.
- **n/a** - does not apply to this product (a CLI has no multi-tenancy).
  Record with a one-clause reason.
- **decide now** - the MVP's code will bake this in soon, so leaving it open
  means it gets decided by accident. Ask.
- **deferred** - can stay open without spreading into code yet. Record with a
  concrete trigger for revisiting: "before the first paid user", "at the
  first multi-device request".

Then ask only the `decide now` items, one at a time, using default plus
reversal cost:

> Settings will live on the device, since the product is personal and login
> is optional. If account sync is ever needed, that is one migration. OK?

Bundle independent ones into a single confirmation when the world picture
makes them all obvious. A new project may have five or six `decide now`
items; that is normal and is the point of this skill.

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
4. **Point the host at it.** Add one line to the project instruction file the
   current host reads - `CLAUDE.md` in Claude Code, `AGENTS.md` in Codex - and
   to both only if the project already has both:
   `Read docs/onward.md before changing shared code, data structures, or public contracts.`
   Do not create the other host's file.
5. **Report** what was decided, what is deferred with its trigger, and what
   is still `open`.

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
| area | status | choice | reversal cost if wrong | revisit when |
|---|---|---|---|---|
| data ownership | decided | device-local | one migration to account sync | first multi-device request |
| billing unit | deferred | - | - | before first paid user |
| multi-tenancy | n/a | personal product | - | - |

When a decision changes, keep the row and add `was: <old> (<date>, <reason>)`
in the choice cell. Do not delete history.

## Shared foundations
| kind | location | use when |
|---|---|---|
| theme tokens | src/styles/tokens.css | any color, spacing, or font |
| modal | src/components/Modal.tsx | any dialog |

Planned shared elements live here from first use. Everything else starts
local and is assessed for promotion at its second real use.

## Deliberately unspecified
- <area left to the implementer>

## Open
- <question asked but not answered>
```
