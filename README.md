# Onward

Build with the next change in mind.

Onward is a development skill for building an MVP and continuing through feature work and maintenance. It helps an agent use product criteria, surface costly choices, reuse existing components, and seek independent judgment at consequential boundaries.

It contains one skill and no runtime, hooks, or review orchestration. Independent review depends on the host's capabilities and permissions. Behavioral benefits remain unverified.

## Install in Codex

Run:

```sh
codex plugin marketplace add https://github.com/ardenspace/onward.git
codex plugin add onward@onward
```

Start a new thread after installation and select Onward from the skill picker. In Codex CLI, invoke it explicitly:

```text
$onward Add a theme preview to the settings screen.
```

The first dogfooding version requires explicit invocation. It does not automatically attach to ordinary requests; `allow_implicit_invocation` is `false`.

To try the instruction body without installation, ask the agent to read [SKILL.md](plugins/onward/skills/onward/SKILL.md) and apply it to a specific task.

## Scope

Onward supports the selected task and its follow-up fixes or resumption. It preserves implementation context, checks fixes in proportion to their impact, and stops stalled verification loops to reassess. It does not automatically expand the task to every issue discovered.

See [DESIGN.md](DESIGN.md) for the design and proposed trials. [HANDOFF.md](HANDOFF.md) preserves development history and local evidence references; those references are not required to use the plugin.
