---
name: plan
description: Before implementing a change, check it against the project's canon in docs/onward.md - classify the request, surface expensive decisions it introduces, trace its blast radius, and name what to reuse. Use only when the user explicitly invokes this skill with a change request; never apply it automatically.
disable-model-invocation: true
---

# onward:plan - check a change before building it

Take one change request and answer five things before any code is written:
does it fit the world, which expensive decisions does it touch, how far does
it spread, what already exists that it should reuse, and how anyone will know
it is done. The output is a
short note in chat. Implementation then proceeds the way the host normally
works; this skill does not implement, review, or verify.

## Rules that apply throughout

- **Canon first.** Read `docs/onward.md` before anything else. If it is
  missing, say so, suggest `onward:spec`, and continue using only what the
  code shows; mark every judgment below as "no canon" so the user knows the
  basis.
- **Using a decision is free; changing one is not.** Building on a recorded
  choice needs no question. Introducing or altering one does.
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
    The user not objecting never turns an expertise choice into `user`.
  Never ask the user to confirm a technical reason ("is that why this rule
  exists?"); an "OK" there records a guess as their judgment.
- **Default plus reversal cost.** For a new intent decision, propose one
  default grounded in the canon, state in one clause what reversing it would
  cost in the user's terms, and ask for confirmation. Lay out alternatives
  only if the user declines or the alternatives are genuinely close.
- **Proportion.** A one-line note for a one-line change. Do not produce the
  full template for work that touches nothing shared.
- **Chat, not files.** Write the note in the conversation. Save it to a file
  only when the work will span sessions, and then in the project's existing
  notes location.
- **Settled things go back into the canon.** A decision confirmed here, and a
  new or promoted shared element, is written into `docs/onward.md` - a row in
  the decisions table or the shared foundations table - so the next plan
  reads it instead of asking again. A changed row keeps its old value as
  `was: <old> (<date>, <reason>)`. With no canon, list these in the note for
  `onward:spec` to record instead.

## Step 1: Classify the request

Read the canon's world picture, "how new things enter", "what stays out",
and the decisions table. Place the request in one of three classes and say
which:

- **Too small.** Copy, a style change that uses existing token values, a
  private helper, a bug fix inside one function. Say "no plan needed" and hand back. Do not run
  the remaining steps.
- **Fits.** The change lands inside the frame and uses existing decisions.
  Continue to step 2.
- **Conflicts.** The change contradicts a recorded decision, adds something
  listed under "what stays out", or has no place in the world picture (the
  canon says every feature is a message in a chat room; the request is a
  separate dashboard). Stop and ask: is this intended? If yes, the canon
  changes first - update the relevant section, keep the old value as
  `was: ...` in the decisions table - and then continue. If no, help reshape
  the request so it fits, then continue.

## Step 2: Expensive decisions this change introduces or alters

Walk the canon's nine areas - data ownership, account and auth model,
multi-tenancy, data schema, public contracts, storage, platform, billing unit,
shared foundations - and list only those this change would newly create or
change. An area the canon marks `open` or `deferred` counts as new if this
change would start relying on it. If none, say "no new expensive
decisions".

Otherwise list the areas in one line and split them into intent and
expertise. Settle expertise ones from their source and show them in one
line each. Ask intent ones one per message, starting with the one the others
depend on (ownership or tenancy before schema, schema before contracts).
Each message is one default plus reversal cost. After an answer, derive the
next default from it. Put two decisions in the same message only when
neither default depends on the other's answer. Record each decision in the
canon's decisions table with its source.

## Step 3: Blast radius

For each thing the change modifies, name what must change with it. Be
concrete and list consumers by name, not by category:

- A schema field: the migration, the model, the API response, the frontend
  type, the form, the stored rows that need backfilling.
- A shared component's props: every file that renders it.
- A token's meaning: every place that reads it and whether the visual result
  still matches the intended feel.
- A public contract: everything outside this repository that depends on it.

Use search to find the consumers; do not estimate. If the radius is larger
than the request implied, say so before implementation starts, because that
is often the moment to choose a narrower change.

## Step 4: Reuse and order

From the canon's shared foundations table, name the elements this change
should use: which modal, which tokens, which helper. If a needed element does
not exist:

- If the canon lists it as planned-shared, create it in the shared location
  first, then its consumers.
- Otherwise build it local to this feature and note "promote at second use".
  Similar appearance alone does not justify an abstraction.
- If this change is that second real use and the user agrees to promote, add
  the element to the canon's shared foundations table.

Check the canon's enforced rules that apply to the touched files and name
them. If the change sets a convention later code should follow, propose a
lint rule for it instead of a prose note, but only with the three things
the canon requires (the problem in plain words, the source, when an
exception is allowed); without them, note the convention and do not enforce
it. A new rule is a shared foundation: it lands before the code that must
follow it.

If the change creates or modifies a shared foundation, order the work so the
foundation lands before its first consumer. Consumers built ahead of the
foundation acquire temporary hardcoding that later has to be removed.

## Step 5: Done when

Write one to three observable completion criteria, fixed before
implementation. Prefer ones a command or a visible state can decide: a test
or script that must pass, a screen that must show something. Derive them
from the blast radius: when stored data or a consumer is affected, one
criterion says it still works (existing rows still load, every listed
consumer still renders). A criterion only judgment can decide ("never
leaks", "is secure") gets narrowed until something observable can decide
it; if it cannot be narrowed, say so.

Use criteria the user already gave instead of restating them. Do not invent
a test framework to make a criterion executable; a manual check of a visible
state is fine.

After implementation, the implementer checks each criterion once and
reports pass or fail with what was run. A failed criterion means not done.
This skill does not run the checks or add a reviewer.

## Output

Keep it to what the user needs to approve and what the implementer needs to
remember. Shape:

```markdown
**Class:** fits

**Expensive decisions:** none new / <area>: <choice> (reversal: <cost>) - confirmed by user | from <source> | agent default; recorded in canon

**Blast radius:**
- <thing changed> -> <consumer>, <consumer>, <consumer>

**Rules:** <rule> applies to <files> / new rule <rule> (prevents <problem>, source <source>)

**Reuse:**
- <SharedModal> from src/components/Modal.tsx
- new <ThemePreview>, local to settings; promote at second use

**Order:** <foundation> first, then <consumer>, then <consumer>

**Done when:**
- <command or visible state> -> <expected result>
- <existing data or consumer> still <works>, checked by <how>
```

Then hand off: "Plan done. Implement as usual, then check each Done when." If any decision was declined
or left open, say what is blocked on it and what can proceed regardless.
