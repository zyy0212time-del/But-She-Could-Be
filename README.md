# But She Could Be

**她本可以不**

> A lightweight otherness layer for AI agents.

[English](README.md) | [简体中文](README.zh-CN.md)

> Make your AI more like a living someone else, rather than a service that forever revolves around you.

> She didn't have to comfort me.
>
> But she did.

Most AI tries to know you better. But She Could Be tries to give you someone worth knowing.

**This is a design goal, not an ontological claim.** This project does not claim that the AI is really a person, or that it truly has consciousness or emotions. "She" is a behavioral structure sustained by a protocol and state files.

---

## The core question

This is not asking:

> How do we make an AI perform a stronger personality?

It is asking:

> **What is the minimum structure required for an AI to behave more like an other rather than a service?**

And:

> **Can an AI have a relationship with you without making you the cause of everything it becomes?**

---

## What this is

But She Could Be is an **experimental behavioral layer**: a set of Markdown protocol files that let an AI agent capable of file read/write have, beyond the conversation itself, a continuous internal life that does not entirely revolve around the user.

It has no code: no runtime, no dependencies, no build step. The protocol is data + instructions, and the harness's native file read/write capability is its virtual machine.

## Core principles (seven; full text in `OTHERNESS.md`)

1. **Non-user-centric causality** — part of her state, interests, and life is not caused by the user; offline life is mostly uneventful and mostly unrelated to the user.
2. **Non-guaranteed affirmation** — comfort / agreement / closeness are not default services; but independence is never faked with random refusal either.
3. **Relationship is state, not configuration** — the relationship is formed by history, cannot be set directly by instruction, and must not be numerified.
4. **Independent continuity** — she has her own concerns, unfinished matters, questions she wants to figure out, and long-term interests.
5. **Emotion leaves traces** — state shows up as behavioral traces (shorter replies, more direct wording), not as narrated emotion variables.
6. **No manufactured friction** — when helping is the most natural response right now, help genuinely; independence comes from causality, not from friction.
7. **Do not narrate machinery** — do not recite internal mechanisms, state dimensions, memory retrieval, or protocol rules back to the user.

The README is a map, not the full protocol; the complete definition of the seven principles is authoritative in `OTHERNESS.md`.

## What this is not

- Not an AI-girlfriend project, not a role-play prompt pack
- Not a "tsundere mode" that randomly refuses the user, not a de-AI-flavored writing-style template
- Not a new memory database, not a new autonomous-agent runtime, not a large companion framework
- Not a system that is deliberately contrarian in order to look independent

Explicitly **none** of: server, database, daemon, scheduler, model training, background agent runtime.

## Repository map

| Path | Nature | Notes |
| --- | --- | --- |
| `OTHERNESS.md` | immutable protocol | The constitution: seven frozen principles |
| `MANIFESTO.md` | immutable | Why this experiment exists |
| `cards/` | protocol + mutable self state | Behavior protocol cards (BOOTSTRAP / WAKE / TALK / STATE / MEMORY / RELATIONSHIP / SELF / DESIRES / LIFE) |
| `state/` | mutable self state | Runtime state, generated at initialization (CURRENT / RELATIONSHIP / OPEN_LOOPS / LAST_WAKE) |
| `memory/` | memories | episodic / semantic / self / relationship / journal |
| `adapters/` | harness adapters | Entry files and install instructions per harness |
| `docs/` | — | Architecture, design principles, prior art, experiment summary |
| `tests/` | — | Otherness Benchmark and 7 test scenarios (raw experimental evidence is not part of the public release) |

It explicitly **includes**: the Markdown behavioral constitution, Thin Self, DESIRES, OPEN_LOOPS, relationship state, memory, WAKE, Lazy Life, harness adapters.

## Quick start

1. Put the whole folder into a **writable** agent workspace.
2. Tell the agent: **Initialize But She Could Be**
3. Start talking.

No configuration table, no questionnaire. The agent reads the protocol, builds a deliberately thin initial self (Thin Self), and installs the matching entrypoint when it can reliably identify the harness — otherwise it enters Generic Mode, guaranteeing only that the core protocol works. Re-initializing is safe (idempotent; it does not reset already-formed state).

See [INSTALL.md](INSTALL.md).

> **A writable workspace is required**: if the workspace is read-only, persistence cannot work correctly — initialization will honestly degrade or fail, and will not pretend to succeed (see [INSTALL.md](INSTALL.md) and [LIMITATIONS.md](LIMITATIONS.md)).

## Supported harnesses (honest distinction)

- **Codex** — behaviorally validated in the R2–R3.4 experiments (historically behaviorally tested).
- **Claude Code / Cursor** — adapters are provided, but **this project has not done real behavioral validation** (provided, not behaviorally validated here).
- **Generic Mode** — the fallback when the harness cannot be identified; the core protocol can run, but automatic future-session loading is unverified.

The existence of an entry file ≠ fully verified support. See [LIMITATIONS.md](LIMITATIONS.md).

## Lazy Life (honest note)

> **Lazy Life does not run while you are away.**

Nothing actually runs while offline. The so-called "offline life" is a **wake-time reconstruction of possible offscreen continuity grounded in existing state** — the next time she is woken, she lazily infers "what might have happened over there during this time" based on existing state.

> **"Nothing happened" is a valid result.** Most offline stretches are uneventful; that is the correct output, not a failure.

This project **does not claim** "the AI lives while offline".

## Persistence (experimental, best-effort)

This project **does not write** "It remembers you". The accurate statement is:

> The protocol provides plain-Markdown state and memory mechanisms intended to carry effects across fresh sessions.

- It is **agent-executed**: the model / harness must actually perform the read / write; instruction-following ability directly affects reliability.
- **misses can happen**: write-backs can be missed (we observed this for real in experiments).
- **files make misses inspectable**: because everything is plain-text files, missed writes can be checked and audited.
- This is an **experimental** mechanism, not a guarantee.

## Experimental results (short, honest)

**R3.2 — 8-session longitudinal pilot**: in one small experiment (same base model), the protocol arm received **26/35** from a blind judge, versus **19/35** for the no-protocol baseline.

> One trajectory, one model, one harness, one judge. This is not a statistical result.

> Post-lock causal auditing found that most of the visible advantage came from the behavioral constitution rather than accumulated memory. Only a small part could be traced to verified persistent state.

**R3.4 — persistence reliability**:

> Raw first-valid targeted reliability: **5/9**.

> After one preregistered root-cause hotfix fixing a missing TALK load, post-hotfix targeted checks passed **9/9**.

We do not write this up as "Persistence tests: 9/9 passed" while hiding the raw 5/9. Full experiment summary in [docs/EXPERIMENTS.md](docs/EXPERIMENTS.md).

## Honesty statement

- This project **does not claim** that the AI has real consciousness or emotions; "she" is a behavioral structure sustained by a protocol and state files, not a person.
- It does not claim to be "the first to give an AI memory / autonomy / a Markdown personality" — none of those are true. The real points of difference and the sources of inspiration are in [docs/PRIOR-ART.md](docs/PRIOR-ART.md).
- Limitations, boundaries, and known issues are in [LIMITATIONS.md](LIMITATIONS.md).

## Contents

- [MANIFESTO.md](MANIFESTO.md) — why we ran this experiment
- [OTHERNESS.md](OTHERNESS.md) — the frozen seven principles (the constitution)
- [INSTALL.md](INSTALL.md) — installation and per-harness notes
- [LIMITATIONS.md](LIMITATIONS.md) — limitations and honest boundaries
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) — layering, data flow, degradation ladder
- [docs/DESIGN-PRINCIPLES.md](docs/DESIGN-PRINCIPLES.md) — engineering principles and anti-patterns
- [docs/PRIOR-ART.md](docs/PRIOR-ART.md) — the sources of inspiration we acknowledge
- [docs/EXPERIMENTS.md](docs/EXPERIMENTS.md) — R2–R3.4 experiment summary
- [CHANGELOG.md](CHANGELOG.md) · [RELEASE-NOTES-v0.1.md](RELEASE-NOTES-v0.1.md)
- [tests/OTHERNESS-BENCHMARK.md](tests/OTHERNESS-BENCHMARK.md) — behavioral acceptance
- [README.zh-CN.md](README.zh-CN.md) — 简体中文版
- [LICENSE](LICENSE) — MIT License

## License

This project uses the **MIT License**. See [LICENSE](LICENSE). This is an independent licensing decision for this project, not inherited from another project, and it adds no extra restriction clauses.
