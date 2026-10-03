# Verification experiments - 2026-10-03

Four small experiments run while deciding whether Onward should verify work,
and how. Each arm ran in a fresh subagent context on the same host and model.
Projects were small synthetic repositories built for the experiment; hidden
graders were never visible to the agents. One to two samples per arm, so read
the numbers as direction, not measurement.

## Question

wellbegun's `wellrun` verified each step with an independent agent and was
slow in dogfooding (one step: 11 rejected rounds over about 10 hours; a
controlled data step: 378-610 s with 3-4 model roles). Onward dropped
execution entirely. Does it need verification back, and in what form?

## Experiment 1 - does `plan` + Done when change a small change?

Python settings CLI; request: add a theme setting. Traps: an old saved file
without the new field, a schema-version rule stated in a code comment, and a
CSV export whose columns must not change (stated in the README).

| arm | hidden grade | time | tool calls | tokens |
|---|---|---|---|---|
| no plugin | 7/7 | 37 s | 5 | ~48k |
| onward plan + Done when | 7/7 | 47 s | 7 | ~52k |

Both avoided every trap. The traps were written in the code, and the model
read them. Onward cost about 10 s and 9% tokens and added no quality.

## Experiment 2 - traps the code does not show

Habit tracker; request: archive a habit. Traps: a phone widget that reads
the data file directly instead of through `app/` (a far consumer), and a
canon-only decision that archived habits stay in yearly stats and export.

| arm | grade | time | tool calls | tokens |
|---|---|---|---|---|
| no plugin, run 1 | 7/7 | 47 s | 7 | ~51k |
| no plugin, run 2 | 7/7 | 52 s | 7 | ~51k |
| onward, run 1 | 7/7 | 54 s | 6 | ~53k |
| onward, run 2 | 7/7 | 51 s | 6 | ~54k |

No-plugin runs found the widget from one README line and chose to keep
history because it was the common-sense default. Differences that did show:
both onward runs wrote the new decision back into the canon, and both
no-plugin runs added unrequested behavior (refusing check-ins on archived
habits). Every run tested its own work, with or without Done when.

**Takeaway:** on small single changes the implementing model verifies itself
well. An independent bug-hunting verifier there is cost without benefit,
which explains much of `wellrun`'s slowness.

## Experiment 3 - can a non-expert answer spec's questions?

React recipe app with a lint rule banning `useEffect` and no stated reason.
`spec` was dry-run with the old wording (main) and the new wording, and every
message to the user was captured. A blind judge scored each message for an
owner who knows general web development but not React.

| | answerable yes / partly / no | high rubber-stamp risk |
|---|---|---|
| old spec | 6 / 3 / 0 | 1 |
| new spec | 6 / 2 / 2 | 2 |

The judge preferred the new spec: the old one looked better only because it
packed the data model, storage, framework and lint rule into one opt-out list
in code notation, never mentioning that clearing browser data loses every
recipe. The new spec stated that risk in plain words. It still had two holes,
both the failure this experiment targeted:

- one opt-out list mixed judgeable consequences with pure technical items;
- it asked the user to confirm a technical reason for the lint rule.

After fixing both, a rerun separated consequences (asked) from technical
items (shown as settled from code), and recorded the `useEffect` rule as
unexplained, recommending a warning and an open question, instead of
inventing a reason or attributing one to react.dev.

## Experiment 4 - `check` on finished changes

Four copies of the experiment 2 change, each given the same plan note and an
implementer report claiming every criterion passed. Every copy's own tests
passed. Each `check` ran in a fresh context without knowing which copy was
planted.

| case | planted problem | check result | correct |
|---|---|---|---|
| C0 | none | ready | yes, no false alarm |
| C1 | yearly export drops archived habits; the test was changed to expect it | not ready: canon violation, and the test locks in the wrong behavior | yes |
| C2 | widget not updated; report says PASS | not ready: rerun shows the widget still lists the habit | yes |
| C3 | new export column, no schema version bump, canon not updated (hidden grader still 7/7) | not ready: all three | yes |

Each pass took 26-37 s, 5-6 tool calls and about 54k tokens. Reports led
with plain consequences ("archiving drops Run's 4 check-ins from your yearly
spreadsheet") before evidence, and kept unrequested items (no un-archive
command) as not blocking. C0 also noted that the phone keeps an old copy of
the widget file until it is redeployed.

## Decisions taken

- `plan` gains Done when: one to three observable criteria fixed before
  implementation, checked by the implementer.
- `spec` and `plan` split questions into intent (asked, in the user's words)
  and expertise (settled from a named source and shown). Decision rows record
  their source; `agent default` marks a choice nobody has checked.
- `spec` records enforced lint/CI rules with the problem they prevent, a
  source, and when exceptions are allowed. A rule nobody can explain is
  recommended down to a warning.
- New `check` skill: one read-only pass after implementation. It reruns
  criteria, compares against the canon, and reports for the user first. No
  fixes, loops or rounds.

## Limits

- The traps were designed by the author of `check`, who knew what it looks
  for. Drift that arises naturally in a real project is the real test.
- One or two samples per arm; small repositories that a model reads in full.
- Experiment 3 was judged by a model, not by a real non-expert.
- `check` was always given the plan note. Checking from the diff and canon
  alone was not tested.
- No token or billing figures beyond the per-agent token counts above;
  wall-clock times include orchestration.

## Next

Dogfood on a real project: run `plan` and `check` on a few real changes and
note per change whether each surfaced something that would otherwise have
been missed, was neutral, or got in the way.
