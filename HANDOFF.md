# Local prototype handoff

Written: 2026-09-13. Read this file first. Do not resume by launching the wellbegun pipeline or reading every historical document.

## Current state

The first local prototype has been implemented. See `DESIGN.md` for the current design and usage example, and `plugins/onward/skills/onward/SKILL.md` for the behavioral instructions. The plugin manifest and explicit invocation policy (`allow_implicit_invocation: false`) are present.

The user requested a new name and English throughout the project for a broader audience and clearer agent instructions. The project instructions and documentation are in English. The user confirmed Onward (`onward`) after rejecting the provisional Roomway name. Onward expresses continued progress from the first MVP through later feature work and maintenance. It replaces the initial plugin identifier `minimal-plugin` and skill identifier `minimal-work`. Both the plugin and skill now use `onward`.

The initial prototype was committed and pushed to `https://github.com/ardenspace/onward.git` on `main` as `1f91325`. The user subsequently authorized GitHub distribution and local installation. Packaging now uses `.agents/plugins/marketplace.json` and `plugins/onward/`; see `README.md` for installation commands. Automatic application remains disabled for the first dogfooding trial. Existing projects remain unchanged, and the plugin has no runtime service to deploy.

Following review, the user approved a narrow stopping-rule improvement: stop and reassess when recurring root causes or an expanding solution make no progress toward completion criteria; require new evidence for a concrete next attempt, or report the unresolved scope and necessary decisions. This does not impose a fixed round cap or guarantee a runtime limit.

Actual behavioral effects and host invocation behavior remain unverified. The next step is to observe the skill on a small real change. In the first shared-foundation trial, verify reviewer startup, the context actually supplied, and independent derivation of checks. Installed explicit invocation and automatic-selection blocking also need behavioral verification; no review orchestration has been added.

The following sections preserve the agreements and evidence that led to the prototype. Historical descriptions of files not yet existing refer to the state before implementation.

## State before implementation

The user agreed to try the first prototype after the direction discussion, then requested a handoff so implementation could continue in the next session. This authorized a small local implementation without restarting planning approval.

At that point the directory contained only `DESIGN.md` and this handoff. No manifest, skill, code, tests, marketplace entry, installation, or remote deployment existed. The plugin-creator and skill-creator instructions had been read, but no generation script or implementation had run.

The initial design draft lacked later agreements on product perspective, consistency across the project, and execution bottlenecks. This handoff captured those later agreements; the design has since been updated.

The user chose `/Users/arden/Documents/dev` as the parent directory. The initial working directory was `/Users/arden/Documents/dev/minimal-plugin`; it was briefly renamed to `roomway` during the English revision, then to `/Users/arden/Documents/dev/onward` after the user confirmed the final name. A provisional identifier was acceptable while the final brand remained undecided.

## Intended value

The goal is an MVP that can be built quickly and remains fast to change and maintain. The plugin should surface considerations and alternatives that the user might otherwise miss. The earlier name wellbegun expressed the value of a good beginning.

Core concerns:

- Surface choices that will be expensive to change, such as data structure, ownership, and public contracts, before introduction. Do not request renewed approval for agreed choices or use of existing contracts.
- Consider product direction and experience criteria across websites, apps, and games. A concept or world should help people make coherent changes: whose experience matters and which good option takes priority when choices conflict. This goes beyond color preference without requiring a long worldbuilding document for every product.
- Support consistency and reuse across the project. Reduce duplicate modals, hardcoded theme colors, and duplicated common functions. Do not register every element or force abstractions for all similar code.
- Review with fresh eyes, using the specific definition of independence below.
- Preserve useful agent judgment and avoid forcing all development into a procedure.

The shared principle is to check consequential choices before they spread and become expensive to revise. Product criteria guide direction, shared structures support consistent implementation, and independent review checks whether the actual result fits. Deliberate changes in concept after learning from users must remain possible.

## Agreed application model

The first experiment applies only to explicitly selected work. Automatic application is a later decision. The skill can participate from the start or join a project already in development.

When joining midway, inspect the product and code evidence relevant to the current task. Do not restart planning or initiate a project-wide audit or rewrite.

For example, a settings-screen request should lead to checking existing modals and themes, identifying device-versus-account ownership if that choice is unresolved and consequential, and independently reviewing changes with substantial shared impact. Unrelated findings must not all become prerequisites for progress.

Start with one short behavioral instruction. Do not prebuild stage commands, a separate runtime, registries, schemas, hooks, or verifier orchestration. Add machinery when actual cases demonstrate a need. Preserve implementation continuity while separating the context used for independent judgment.

## What fresh eyes means

Four posts about the user's loopspace/Ornith experiments informed this definition. The essential property is a reviewer who has not inherited the implementation process and independently decides what needs checking. A different model provider, fresh instance, or role label does not establish independence.

Use user requirements, agreed product criteria and contracts, and the actual working tree without inheriting implementation conversations or self-assessments. The implementer's tests or reference implementation can carry the same error and must not be treated as an answer key. One experiment used an ostensibly independent reference that copied the implementation's parser and agreed with the same error.

The proposed approach, now reflected in the skill, is to derive checks and failure scenarios from requirements and code first, then compare existing tests for omissions and incorrect expectations. This does not mean hiding implementation code or permanently withholding tests.

Report test results, implementer self-review, and independent judgment separately. Fresh eyes cannot guarantee detection of every bug. The experiments also showed distinct benefits from test density targeting state interactions and from execution-environment isolation.

The independent review mechanism remains unspecified and follows host capabilities and permissions. This handoff does not automatically authorize additional agents or separate experiment runs. If necessary independent review is unavailable, leave the boundary unverified and continue work that does not depend on its outcome. Do not present self-review as a substitute that achieved independence.

## KBO bottlenecks and design constraints

The user described wellrun taking days and asked whether substantial parts should be abandoned. KBO and wellbegun records were inspected read-only. The full raw usage and conversation logs were not recounted, and no comparative execution was performed.

### Initial cycle

Git records show the first implementation commit at 2026-08-24 22:06:31 +0900 and the execution-completion commit at 2026-08-25 13:54:14 +0900, approximately 15 hours 48 minutes apart. This excludes planning and includes waiting; it is not pure implementation time. It neither explains nor disproves the user's experience of several days.

`docs/wellbegun-dogfood-proposal.md` records 19 steps across 5 phases, approximately 3.4 million subagent tokens, usage limits reached twice in one day, three interruptions, and roughly 250,000-300,000 tokens lost or spent reworking. It reports approximately 2 million implementation tokens and 1.37 million verification tokens. Implementation input per call grew from about 60,000 to 110,000-130,000 tokens.

The document's stated total of 36 calls conflicts with a line-item sum of 39. Do not present an exact call total or billing amount as established.

Structural costs included new implementers relearning code and conventions at each step, cumulative step/phase/whole-system reviews, and similar journey probes being created, deleted, and recreated by later reviewers.

There were real protective effects: integration review found a team-theme inheritance defect, and whole-system review found a mismatch between the iOS deployment target and Firebase requirements. Later wellbegun versions consolidated regression execution and retained successful probes as reusable assets. Do not describe initial-cycle costs as unchanged defects in version 0.6.0.

### Verification escalation in the second cycle

Verification of step 4.1, location classification, reached 12 rounds. An analysis at round 11 already described about 10 hours. In that analysis, a coordinate-egress hook grew from 35 to 1,376 lines, with 452 lines of supporting files and 2,678 lines of boundary tests.

The product decision logic stopped changing after early fixes, while tooling syntax handling, false positives, and documentation claims continued expanding. Records disagree on whether the last product change was in round 2 or 3; do not assert an exact boundary.

Even valid findings can turn a protective mechanism into a separate development project attached to the original feature's completion criteria. This motivated contract-based judgment, a round cap, and limits on supporting-artifact expansion in wellbegun 0.4.1.

The key design decision is to separate discovering a missed issue from making it mandatory for the current MVP. Reviewers may freely identify relevant risks, while assessing requirement violations, material risks introduced by the change, and improvement suggestions to determine execution scope.

Do not dismiss an important defect simply because it falls outside the stated contract. Do not automatically block completion on every finding either.

The user agreed to proceed after the following defaults were proposed for removal:

- A new implementer at every step and three fixed review layers.
- Fixing every discovered issue within the current task.
- Repeating a full blind review after each fix.
- Enforcing every rule through a dedicated hook and recording every judgment in a registry.

Initial independent judgment and fix verification serve different purposes. Review of a fix can focus on the finding and affected behavior, broadening when shared contracts or structure change.

Reducing full reviews also reduces opportunities to detect defects. Do not promise unconditional speed gains without a quality tradeoff. Completion conditions are necessary, but no fixed round count, severity taxonomy, or review execution implementation has been agreed.

Do not copy rules such as treating unchanged product code as automatic success. Tests or protective mechanisms can themselves be mandatory parts of a contract.

## Comparison reference and initial observations

The user proposed https://github.com/obra/superpowers as a comparison reference. The public main branch inspected on 2026-09-13 also used fresh implementers and reviews per task, so it should not be described as inherently lightweight.

Useful ideas included grouping small similar tasks, avoiding repeated inspection of the same code, limiting fix reviews to affected scope, bounding fix loops, and handling incidental findings. There was no agreement to always run superpowers alongside this plugin or copy its implementation.

Initial trial candidates:

1. A small UI change in an existing project: reuse shared elements without unnecessary questions, process documents, or extra agents when criteria are clear.
2. A settings feature or other product/data ownership choice: surface only unresolved decisions that change the outcome, without reopening agreements.
3. A shared foundation and its consumers: independent review derives omitted conditions at the relevant boundary without inheriting the implementation narrative.
4. A second change after the MVP: observe whether changing a shared modal or theme reduces duplicate edits and omissions.
5. Resumption: reconcile notes with actual state without reimplementing completed work.

Do not begin with a large benchmark. A baseline consisting only of short instructions also has comparison value. Distinguish elapsed time, input volume, calls, questions, process documents, and repeated checks from actual protective outcomes. Separate task cost from plugin overhead. Unmeasured tokens and monetary amounts remain unknown.

Behavioral benefits and maintenance savings are currently unverified.

## Original implementation starting point

The first implementation followed this agreed direction:

1. Check the directory and applicable AGENTS.md instructions; update the older design with later agreements.
2. Create minimal local packaging and one explicitly invoked instruction. Codex packaging fits the current host; do not prebuild multiple-host support.
3. Use relevant available creator skills as needed. Do not run this experiment through wellbegin, wellspec, wellplan, or wellrun.
4. Validate syntax, manifest, and invocation policy, separating structural checks from behavioral evidence.
5. Deliver a local artifact that the user can read and try. A provisional name need not block implementation.

Explicit invocation is the user's chosen experiment model and should be reflected in host invocation policy where supported.

Installation, marketplace registration, replacement, and deployment have not been requested. A creator skill's default registration workflow must not expand that scope. Git initialization and remote hosting are not prerequisites.

## Read supporting evidence only when needed

- `/Users/arden/Documents/dev/wellbegun/docs/2026-09-13-minimal-plugin-handoff.md`: initial handoff and experiment boundaries.
- `/Users/arden/Documents/dev/kbo-away-fans/docs/wellbegun-dogfood-proposal.md`: initial cost and defects detected.
- `/Users/arden/Documents/dev/wellbegun/docs/2026-09-05-wellrun-round-cap-notes.md`: step 4.1 escalation analysis.
- `/Users/arden/Documents/dev/kbo-away-fans/.wellbegun/cycles/02/run.md`: final 12-round record; approximately 276 KB, so inspect only relevant sections.
- `/Users/arden/Documents/dev/ardenspace/src/content/ko/blog/same-mind-blind-spots.mdx`: experiment revising a model-lineage explanation toward reviewer setup.
- In the same blog directory: `my-harness-lost.mdx`, `experiment-lied-to-me.mdx`, and `when-context-overflows.mdx`, covering the initial comparison, environment contamination, and limits of scale and excessive structure.
- https://github.com/obra/superpowers/blob/main/skills/subagent-driven-development/SKILL.md
- https://github.com/obra/superpowers/blob/main/skills/writing-plans/SKILL.md

These local evidence paths belong to the author's environment. They preserve provenance and are not prerequisites for using the skill. Existing wellbegun, KBO, and ardenspace projects remain read-only references. This work does not include replacing or migrating the established plugin, remote deployment, or intervention in other ongoing sessions.

## Suggested next-session prompt

> Read this project's HANDOFF.md first and continue from the actual state. Keep the agreed product perspective, costly-decision checks, reuse, and independent judgment. Use one explicitly selected skill and avoid introducing repeated implementation or unbounded review. Preserve existing projects as reference material.
