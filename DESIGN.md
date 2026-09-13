# Local prototype design

Status: first implementation completed on 2026-09-13 after agreement on direction. The user confirmed Onward (`onward`) as the name for both the plugin and its single skill. The name expresses continued progress from the first MVP through later feature work and maintenance. See [HANDOFF.md](HANDOFF.md) for the agreements and their background.

## Purpose

Help build an MVP that remains fast to change and maintain. Bring a small set of useful judgments into ordinary development: product intent, costly choices, shared implementation, independent review, and continuity across sessions.

The user requests work rather than managing a pipeline. Existing wellbegun, KBO, and ardenspace projects remain reference material. Copying their implementations, migration, replacement, installation, marketplace registration, and deployment are outside this prototype's scope.

## Implementation

The prototype contains one behavioral skill, packaged locally for Codex:

- `.codex-plugin/plugin.json`
- `skills/onward/SKILL.md`
- `skills/onward/agents/openai.yaml`, with `allow_implicit_invocation: false`

Apply only to a task the user explicitly selects and its follow-up fixes or resumption. When joining a project in progress, read the relevant product and code evidence and continue from its current state.

Separate stage commands, a runtime, registries, schemas, hooks, and verifier orchestration should be added only when actual use demonstrates a need.

## Decision criteria

Identify whose experience matters and which priorities should win when good choices conflict. Product concept and worldbuilding matter where they guide that experience; they do not require a long document. Deliberate changes in direction based on user learning remain possible.

Check data ownership, storage, public contracts, and other costly choices before introducing or spreading them. Existing agreements and authority are sufficient grounds to proceed. Using an agreed shared API does not require the approval needed to change its contract.

Find relevant shared UI, themes, tokens, functions, and consumers before adding new elements. Reuse suitable elements and consider the consumer impact of variations. Do not register every element or abstract code merely because it looks similar.

| Moment | Default behavior | Reason for additional involvement |
| --- | --- | --- |
| Start work | Derive completion criteria and proceed from relevant evidence. | A missing choice would change the result. |
| Introduce or change a decision | Use existing contracts and authority. | A costly new choice lacks necessary evidence or authority. |
| Propagate a shared foundation | Match checks to change impact. | An error could spread to multiple consumers; obtain independent judgment first. |
| Pause or hand off | Record only continuation information missing outside the conversation. | Work spans sessions; update at consequential boundaries to support recovery. |
| Finish | Report completion and verification evidence. | State the scope of failures, unverified boundaries, or blockers accurately. |

## Independent judgment

Keep test results, implementer self-review, and independent judgment distinct. A reviewer must not inherit the implementation conversation, self-assessment, or expected conclusions. Provide the user requirements, agreed product criteria and contracts, and actual working tree.

The reviewer derives checks and failure scenarios from requirements and code before comparing them with existing tests. Tests and reference implementations may contain the same error as the implementation. A new instance, provider, or role name alone does not establish independence.

The execution mechanism remains unspecified and depends on host support and session authority. If necessary independent review is unavailable, leave the boundary unverified, hold work dependent on that result, and continue independent work.

## Findings and completion

Separate discovering a problem from making it mandatory for the current task. Assess requirement violations, material risks introduced by the change, and improvement suggestions using evidence and impact. Surface important defects outside the contract without automatically expanding completion criteria.

Preserve implementation context and avoid fixed review layers. Check fixes against the finding and affected behavior; widen review when shared contracts or structure change, or when new evidence warrants it. Fewer full reviews also mean fewer detection opportunities.

Finish when requirements and necessary checks are satisfied and material risks introduced by the change are resolved. If only review tooling or supporting documents keep expanding, assess whether that work is necessary to establish a current requirement. Defer optional work; report unresolved mandatory checks as incomplete. No fixed round count or severity taxonomy is defined.

If failures with the same root cause recur, or the attempted solution keeps expanding, without progress toward completion criteria, stop that loop and reassess. Resume it only with a concrete next attempt supported by new evidence; otherwise report unresolved scope and necessary decisions. Work independent of that outcome may continue. This behavioral stopping rule does not guarantee a runtime limit.

## Continuity

Use an existing handoff location before creating a new one. If none is suitable, create a single short note with completed scope, next action, blockers or decisions, and verification evidence tied to the checked state. No fixed filename or schema is required.

Reconcile notes with the actual state when resuming. Avoid repeating valid completed work or checks. Small tasks completed in one session need no process document.

## Initial behavioral trials

- Small UI edit: reuse existing elements and finish appropriate checks without unnecessary questions, documents, or additional agents when criteria are clear.
- Shared foundation: surface unresolved costly choices before introduction and independently detect a seeded boundary defect before propagation. Record a missed defect as a failed trial.
- In that shared-foundation trial, also check how the reviewer is started, which context it actually receives, and whether it derives checks before consulting existing tests. A configured role or structural validation alone is insufficient evidence of independence.
- Resumption: recover completed scope and next steps, recheck changed evidence, and preserve the distinction between complete and unverified.
- Settings addition: surface only unresolved ownership choices that change the outcome; do not revisit existing agreements.
- Second change after the MVP: observe duplicate edits and omissions when modifying a shared modal or theme.

Possible comparisons are no plugin, the existing wellbegun plugin, and this prototype, using equivalent tasks and evaluation criteria in separate contexts. Begin with small trials. Measure elapsed time, added input bytes, calls, questions, document writes, repeated checks, and actual defects found. Separate task cost from plugin overhead. Unmeasured tokens and monetary cost remain unknown. This document does not authorize comparison runs or additional agents.

## Local use and verification

To test the instructions without installation:

> Read `skills/onward/SKILL.md` from this plugin directory and apply it to this task. In [target project path], implement [small change request].

This tests the instruction body. In an installed, discoverable environment, the intended explicit invocation is `$onward`. Creating these files does not register that command in the current session.

On 2026-09-13, the initial prototype passed the creator skills' `validate_plugin.py` and `quick_validate.py`. The default Python lacked PyYAML, so they were run with `uv run --no-project --with pyyaml python`. The explicit invocation policy value was also checked.

After the English revision and rename to Onward, both validators passed again. A scan found no remaining Korean text in the five project files. Historical identifiers remain only in the handoff's provenance records.

These are structural checks. Actual project behavior, host invocation and automatic-selection blocking, independent defect detection, and maintenance savings remain unverified. Observe a small real task next. Automatic application, multiple hosts, and review execution machinery remain future decisions.
