# Rust Minecraft Server Engine

## Project

This repository contains a long-term Rust implementation of a high-performance Minecraft Java Edition-compatible server engine.

The project must be developed incrementally.

Do NOT generate the entire project at once.

---

# Development Workflow

Every meaningful change must follow this sequence:

1. Inspect the current repository.
2. Understand the existing architecture.
3. Identify the smallest useful implementation step.
4. Explain the engineering rationale briefly.
5. Implement the change.
6. Run relevant tests/checks.
7. Inspect the resulting diff.
8. Fix problems.
9. Update documentation when appropriate.
10. Create a focused Git commit.
11. Report the commit hash and what changed.
12. Stop and wait for the next task.

Do not silently make large architectural changes.

---

# Reasoning / Engineering Notes

Do NOT expose private chain-of-thought.

Before implementation, provide a concise engineering summary containing:

* Goal
* Current problem
* Proposed approach
* Important trade-offs
* Files that will change
* Tests that will be run

After implementation, provide:

* What changed
* Why it changed
* Tests/checks performed
* Performance implications
* Remaining concerns

Keep these explanations concise and technical.

---

# Commit Policy

Make small, atomic commits.

One commit should represent one logical change.

Preferred format:

feat(protocol): add varint decoder

fix(network): handle malformed packet lengths

test(protocol): add varint edge cases

refactor(world): separate chunk storage

docs(architecture): document protocol boundary

perf(chunk): reduce chunk allocation overhead

chore(ci): add Rust test workflow

Do NOT create giant commits containing unrelated changes.

Never commit generated build artifacts.

Never commit:

* secrets
* API keys
* passwords
* `.env` files containing secrets
* target/
* IDE-specific temporary files

---

# Before Every Commit

Run the appropriate checks.

At minimum, when applicable:

```bash
cargo fmt --check
cargo check --workspace
cargo test --workspace
```

For larger changes also use:

```bash
cargo clippy --workspace --all-targets --all-features
```

Do not commit if the project is knowingly broken unless the commit is explicitly documenting an intermediate broken state and I have requested that behavior.

---

# Git Safety

Do NOT:

* force push
* reset/delete commits
* rewrite shared history
* use `git push --force`
* delete branches
* modify unrelated files

without explicit permission.

Before pushing, inspect:

```bash
git status
git diff
git log -5 --oneline
```

Do not push automatically unless explicitly instructed.

Local commits are encouraged after each completed logical milestone.

---

# Architecture Rules

Keep Minecraft protocol handling separate from the internal game engine.

The engine must use canonical/internal representations rather than raw protocol packet IDs.

Preferred conceptual flow:

Minecraft Client
→ Protocol
→ Version Adapter
→ Canonical Representation
→ Game Engine

and:

Game Engine
→ Canonical Representation
→ Version Adapter
→ Protocol
→ Minecraft Client

---

# Version Support

Do not implement every Minecraft version immediately.

Start with one clearly defined protocol version.

The version abstraction must exist before adding additional versions.

Never pretend that a version is supported when it has not been tested.

---

# Performance

Performance claims must be measured.

Do not claim:

"supports 500 players"

unless a reproducible benchmark demonstrates it.

Track where appropriate:

* TPS
* tick duration
* CPU usage
* memory usage
* allocations
* network throughput
* packet rate
* loaded chunks
* entities
* chunk load latency
* chunk generation time

Avoid premature optimization.

Correctness comes before micro-optimization.

---

# Rust

Prefer safe Rust.

Avoid `unsafe` unless there is a measurable technical reason.

If unsafe code is necessary:

1. Explain why.
2. Isolate it.
3. Document safety invariants.
4. Add tests.

Avoid unnecessary dependencies.

Prefer simple, maintainable designs.

---

# Testing

New functionality should include appropriate tests.

Important areas include:

* packet codecs
* malformed packets
* networking
* chunk serialization
* world storage
* entity systems
* concurrency
* version compatibility

Network-facing parsers should be robust against malformed input.

---

# Documentation

Important architectural decisions should be documented.

Use:

```text
docs/
├── architecture/
├── protocol/
├── performance/
└── decisions/
```

For significant architectural decisions, create a short Architecture Decision Record.

Example:

```text
docs/decisions/0001-canonical-protocol.md
```

---

# Current Development Rule

Work incrementally.

Never implement multiple major subsystems without stopping for review.

The project should remain understandable at every stage.
