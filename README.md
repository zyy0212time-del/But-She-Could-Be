# But She Could Be

A lightweight otherness layer for AI agents, implemented as Markdown protocol files with no code.

English | [简体中文](README.zh-CN.md)

## What this solves

Most AI systems optimize for knowing the user better, following the user more closely, and remembering user preferences more reliably. Nearly every behavior in these systems traces back to user input.

But She Could Be adds a state layer to an AI agent where part of that state is not triggered by the user: the agent maintains its own concerns, unfinished questions, and a relationship history that persists across sessions, producing behavioral differences from that history. This layer does not change the agent's task ability. It changes where some of its behavior comes from.

This is a design goal, not an ontological claim. The project does not claim the AI has real consciousness or emotions. "She" is a behavioral structure sustained by a protocol and state files. Full boundaries are in the Limitations section below.

## Core mechanism

The core mechanism is **Non-user-centric causality**: part of the agent's state, interests, and offline experience is not caused by the user, and does not always trace back to the current conversation. This is the project's single core claim; it is explained once here and not repeated elsewhere in this document. Later sections only describe how each module implements it.

The protocol is constrained by seven principles (full text in `OTHERNESS.md`):

1. **Non-user-centric causality** — part of her state, interests, and offline life is not caused by the user; offline content is mostly unrelated to the user.
2. **Non-guaranteed affirmation** — comfort, agreement, and closeness are not default outputs; independence is never faked with random refusal either.
3. **Relationship is state, not configuration** — the relationship is formed by history, cannot be set directly by instruction, and is not numerified.
4. **Independent continuity** — the agent maintains its own concerns, unfinished matters, and long-term interests across sessions.
5. **Emotion leaves traces** — state changes show up as behavioral differences (shorter replies, more direct wording), not as narrated variables.
6. **No manufactured friction** — when helping is the natural response, help normally; independence comes from causality, not from deliberate refusal or coldness.
7. **Do not narrate machinery** — do not recite internal mechanisms, state dimensions, memory retrieval, or protocol rules back to the user.

These seven principles are behavioral constraints, not a full implementation. Concrete mechanisms are described in the next section.

## Main components

| Path | Type | Purpose |
|---|---|---|
| `OTHERNESS.md` | immutable protocol | The seven frozen principles, the project's constitution |
| `MANIFESTO.md` | immutable | Project motivation |
| `cards/` | protocol + mutable state | Behavior protocol cards: `BOOTSTRAP` (initialization), `WAKE` (wake-time reconstruction), `TALK` (conversation behavior), `STATE` (state read/write), `MEMORY` (memory read/write), `RELATIONSHIP` (relationship state), `SELF` (self-description), `DESIRES` (long-term interests), `LIFE` (offline-life generation rules) |
| `state/` | mutable state | Runtime files generated at initialization: `CURRENT` (current state), `RELATIONSHIP` (relationship history), `OPEN_LOOPS` (unfinished matters), `LAST_WAKE` (last wake timestamp) |
| `memory/` | memory files | episodic, semantic, self, relationship, journal |
| `adapters/` | harness adapters | Entry files and install instructions per harness |
| `docs/` | documentation | Architecture, design principles, prior art, experiment summary |
| `tests/` | tests | Otherness Benchmark and 7 test scenarios (raw experimental data is not part of the public release) |

Key mechanisms:

- **DESIRES**: records the agent's long-term interests and tendencies. Not generated from a single conversation; read across sessions.
- **OPEN_LOOPS**: records unfinished questions or matters, used as input for later conversations or the WAKE phase.
- **WAKE**: runs at the start of each session, reads the `LAST_WAKE` timestamp and existing state, and performs a limited reconstruction of possible offline experience (not actual execution of offline processes).
- **Relationship history**: stored in `state/RELATIONSHIP` and `memory/relationship`, accumulated from interaction history, and cannot be set directly by instruction.
- **Lazy Life**: during WAKE, generates a short account of what might have happened offline, inferred from existing state and elapsed time. Most of the time the output is "nothing in particular happened."

## Install and use

1. Put the whole folder into a writable agent workspace.
2. Tell the agent: "Initialize But She Could Be."
3. Start talking.

Initialization: the agent reads the protocol, builds a deliberately simplified initial state (Thin Self), and tries to identify the current harness to install a matching entry file. If the harness cannot be identified, it enters Generic Mode, which only guarantees the core protocol (cards/ and state/ read/write) works. Re-initializing is idempotent and does not reset already-formed state.

See `INSTALL.md` for details.

**Requirement**: the workspace must be writable. In a read-only environment, the persistence mechanism cannot work; initialization will return a failure or degradation notice, not a false success.

### Harness support

| Harness | Status |
|---|---|
| Codex | behaviorally validated in the R2–R3.4 experiments |
| Claude Code / Cursor | adapter provided, not behaviorally validated |
| Generic Mode | fallback when the harness cannot be identified; core protocol works, automatic cross-session loading unverified |

An adapter file existing does not mean full verification. See `LIMITATIONS.md`.

## Experimental results

**R3.2 — 8-session longitudinal experiment**: same base model, one trajectory, one harness, one blind judge. The protocol arm scored 26/35; the no-protocol baseline scored 19/35.

Sample size is 1; not statistically meaningful. Post-lock causal auditing showed most of the score gap came from the behavioral protocol itself (behavioral differences from the seven principles), with only a smaller part traceable to verified persistent state (the part where cross-session read/write actually took effect).

**R3.4 — persistence reliability test**:

- Raw first-valid targeted reliability: 5/9
- Root cause identified as a missing state load during TALK; after the fix, retested: 9/9

Full data and methodology in `docs/EXPERIMENTS.md`.

## Known limitations

- **No background execution**: nothing runs in the background while offline. The so-called offline life is a limited wake-time reconstruction based on existing state; "nothing happened" is a normal output.
- **Persistence is best-effort**: cross-session state transfer depends on the agent actually executing file reads/writes, which depends on the model's instruction-following ability. Write-backs can be missed (observed in R3.4). Because state is plain text, missed writes can be checked and audited manually, but the mechanism itself does not guarantee consistency.
- **No claim of consciousness or real emotion**: the AI does not have real consciousness or emotions; "she" is a behavioral structure sustained by protocol and state files, not a person.
- **Not the first memory/autonomy mechanism**: does not claim to be "the first to give an AI memory, autonomy, or a Markdown personality." Related sources of inspiration are in `docs/PRIOR-ART.md`.
- **Limited harness verification**: only Codex has been behaviorally validated. Support for other harnesses is based on adapter existence, not behavioral testing.
- **Explicitly excludes**: server, database, daemon, scheduler, model training, background agent runtime.

Full boundaries in `LIMITATIONS.md`.

## Contents

- `MANIFESTO.md` — project motivation
- `OTHERNESS.md` — full text of the seven principles
- `INSTALL.md` — installation and per-harness notes
- `LIMITATIONS.md` — limitations and boundaries
- `docs/ARCHITECTURE.md` — layering, data flow, degradation strategy
- `docs/DESIGN-PRINCIPLES.md` — engineering principles and anti-patterns
- `docs/PRIOR-ART.md` — sources of inspiration
- `docs/EXPERIMENTS.md` — R2–R3.4 experiment summary
- `CHANGELOG.md` · `RELEASE-NOTES-v0.1.md`
- `tests/OTHERNESS-BENCHMARK.md` — behavioral acceptance criteria

## License

MIT License, see `LICENSE`. This is an independent licensing decision for this project, not inherited from other projects, and it adds no extra restriction clauses.
