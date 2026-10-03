---
name: check
description: After a change is implemented, check the result once against the project's canon in docs/onward.md - rerun the Done when criteria, find expensive decisions the change made without declaring them, and report in words the user can judge. Use only when the user explicitly invokes this skill; never apply it automatically.
disable-model-invocation: true
---

# onward:check - check a finished change once

Implementation happened however the host normally works. This skill looks at
the result once and answers three things: do the completion criteria really
pass, did the change make or alter an expensive decision without saying so,
and what does that mean for the user. The output is a short report in chat.

It is not a second implementer and not a bug hunt. The implementing agent
already tests its own work and is usually right about ordinary correctness.
This check covers what that agent is worst placed to see: whether its own
report holds when rerun, and whether it drifted from decisions recorded
before it started.

## Rules that apply throughout

- **Read-only.** Do not edit code, tests, configuration, or the canon. Run
  commands only on copies or in temporary locations when they would write
  project data.
- **One pass.** Report and stop. Do not fix, do not loop. If the user fixes
  something and asks again, recheck only the listed findings, not the whole
  change.
- **Evidence or nothing.** Every finding cites what shows it: a command and
  its output, or a file and line. A suspicion without evidence is not a
  finding; leave it out.
- **Bound to criteria and canon.** A finding blocks only if it breaks a Done
  when criterion, a recorded decision, a public contract, an enforced rule,
  or something listed under "what stays out". Everything else - style,
  better designs, extra tests you would have written - goes under "not
  blocking" in one line each, or is left out.
- **Fresh eyes when possible.** Prefer running in a context that did not
  implement the change: a new session or a subagent. If this context did the
  implementation, say so in the report; it is still useful for rerunning
  criteria and comparing against the canon, but it is not an independent
  review.
- **Proportion.** A one-line change gets a one-line report.

## Step 1: Gather

- The canon: `docs/onward.md`. If missing, say so and check only the Done
  when criteria and enforced rules; mark the report "no canon".
- The change: the diff against the base the user names, or the uncommitted
  changes, or the current branch against its merge base. List the changed
  files.
- The Done when criteria from the plan note, if it is in this conversation
  or the user provides it. If not, derive one to three from the diff and the
  canon the way `onward:plan` would, and say they were derived.
- The implementer's report, if available. Treat its claims as things to
  verify, not as evidence.

## Step 2: Rerun Done when

Run each criterion yourself. A command criterion passes only on its actual
exit status and output now, not on the implementer's report. Use copies of
any data a command would change. A criterion that needs a person to look at
a screen is marked "needs you to look" with exactly what to open and what
should be there. Record each as pass, fail, or needs you to look.

## Step 3: Compare against the canon

For each changed file, ask whether the change:

- **Alters a recorded decision** in the decisions table - data shape,
  ownership, storage, contracts - beyond what the decision allows. A schema
  change that skips the recorded migration rule counts.
- **Breaks a public contract** - an exported format, an API shape, a URL
  others depend on.
- **Adds something under "what stays out"** or conflicts with "when good
  options conflict".
- **Bypasses a shared foundation** - a hardcoded value where the canon
  names a token or helper, a second copy of a listed shared element.
- **Made a new expensive decision without recording it** - a new stored
  field, a new contract, a new dependency of the platform kind - with no
  matching row added to the canon.
- **Weakens an enforced rule** - new disable comments, especially without a
  reason, or configuration that lowers a rule. Run the project's lint if it
  is configured and fast.

Search for consumers of anything changed in a shared shape, the way
`onward:plan` traces blast radius, and confirm each still works or was
updated.

## Step 4: Report

Lead with what the user needs to know, in their words: what now works,
what might break for them, and what needs their decision. Then the
evidence for the implementer.

```markdown
**Result:** ready / not ready / needs your decision

**For you:**
- <plain-language consequence, e.g. "Old saved data still opens.">
- <plain-language risk, e.g. "Archived habits now vanish from your yearly
  export, which the canon says must keep every habit's history.">

**Done when:**
- <criterion> - pass / fail / needs you to look: <what ran, result>

**Canon:**
- <finding> - <decision or contract it breaks> - <evidence: command output
  or file:line>
- canon not updated: <new decision the change made> (record it, or undo it)

**Not blocking:**
- <one line each, or "none">

Checked in: <fresh context | the implementing context>
```

"Ready" means every criterion passed and no canon finding blocks. "Needs
your decision" means the change works but makes or alters an intent
decision only the user can accept; phrase it as a consequence, not a
technical question. Then stop.
