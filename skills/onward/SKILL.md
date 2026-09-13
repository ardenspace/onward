---
name: onward
description: Carry out explicitly selected development work while checking product intent, costly decisions, reuse, and boundaries that need independent review. Apply only when the user selects this skill for the task.
---

# Onward

Complete the requested work while surfacing consequential choices before they become expensive to change.
Apply to the selected task and its follow-up fixes or resumption. Do not carry it into unrelated tasks automatically.

## Make decisions in the current task

- Derive completion criteria from the request and existing agreements. Read the product and code evidence relevant to this task. Join an existing project where it is; do not default to restarting planning, auditing the whole project, imposing fixed stages, or creating process documents.
- Find the product criteria that identify whose experience matters and which priorities should win when good options conflict. Use the product concept or worldbuilding where it informs that experience. Ask only about missing choices that would change the result; use grounded assumptions for minor implementation choices. Accept deliberate changes in direction based on learning from users.
- Before introducing or spreading costly choices such as data structure, ownership, storage, or public contracts, check existing evidence and downstream effects. Proceed when agreements and authority are sufficient. Otherwise, make the alternatives and their consequences concrete and request only the missing decision. Do not seek renewed approval for agreed choices or use of existing contracts.
- Before adding an element, find relevant shared UI, themes, tokens, functions, and their consumers. Reuse them when suitable. If a variation is needed, consider its impact on existing consumers. Similarity alone does not justify an abstraction or a separate registry.

## Use fresh eyes at consequential boundaries

Match checks to the impact of the change. Do not attach extra agents or fixed review layers to a small, local edit.
When an error in a new or changed shared foundation or contract could spread to multiple consumers, treat that boundary as requiring independent judgment before propagation. Preserve implementation context; do not restart the implementer for every review.

The review mechanism depends on host capabilities and session permissions. This skill does not itself authorize additional agents or separate runs. When independent review is available:

- Give a reviewer the user requirements, agreed product criteria and contracts, and the actual working tree in a context that does not inherit the implementation conversation, self-assessment, or expected conclusions. A different model provider or role name does not establish independence.
- Have the reviewer derive checks and failure scenarios from the requirements and code first, then compare them with existing tests. Treat the implementer's tests and reference implementations as evidence to examine, including omissions and incorrect expected results, rather than an answer key.
- Distinguish test results, implementer self-review, and independent judgment. If independent review is unavailable, mark that boundary as unverified and continue work that does not depend on its outcome. Do not label self-review as independent review.

## Handle findings and finish

- Assess each finding by evidence and impact: a violation of current requirements, a material risk introduced by this change, or an improvement suggestion. Surface important defects even outside the stated contract, but do not automatically add every finding to the current completion criteria. If scope must expand, make the necessary change concrete and request the user's decision.
- Check fixes against the reported issue and the impact of the change. Broaden review when a fix changes shared contracts or structure, or new evidence warrants it. Do not repeat a full blind review after every fix.
- If failures with the same root cause recur, or the scope of the attempted solution keeps growing, without progress toward completion criteria, stop the current loop and reassess the approach. Continue only with a concrete next attempt supported by new evidence; otherwise report the unresolved scope and any decision needed. Continue unrelated work that does not depend on that outcome.
- Finish when current requirements and necessary checks are satisfied and material risks introduced by the change are resolved. If work keeps expanding only the review tools or supporting documents, assess whether that expansion is necessary to establish a current requirement. Defer optional improvements; report the reason and incomplete scope when a necessary check remains unresolved. Unchanged product code is not proof of success.
- Report completed scope, verification evidence, and remaining risks or unverified boundaries concisely. If fewer full reviews mean fewer opportunities to find defects, do not claim an unconditional speedup with no quality tradeoff.
- For interrupted work or work spanning sessions, record missing continuation information in the project's existing handoff location: completed scope, next action, pending decisions, and what was checked against which state. Create one short note only if no suitable location exists. On resumption, reconcile it with the actual state and avoid repeating valid completed work or checks.
