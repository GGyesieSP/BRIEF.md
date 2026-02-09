# BRIEF.md — An Open Context File for Projects

## The Problem

Modern creative and technical work suffers from **context decay**.

We switch tools, pause projects, collaborate asynchronously, and increasingly rely on AI assistance. What gets lost is not files or parameters, but *intent*: why a project exists, what constraints matter, and which decisions were made and why.

Most tools preserve state. None preserve meaning.

## What Is BRIEF.md?

BRIEF.md is a simple Markdown file placed at the root of a project that captures:

- **What This Is** — a clear description of the project
- **What This Is NOT** — boundaries and constraints
- **Why This Exists** — motivation and intended outcome
- **Key Decisions** — choices made, with rationale and rejected alternatives
- **Open Questions** — what's unresolved, and what should stay unresolved

Think of it as a README for *intent*, not implementation.

## Quick Example

```markdown
**Project:** Echo Valley
**Type:** song
**Created:** 2026-01-15

# What This Is
A folk song about small-town restlessness told through late-night train imagery.

# What This Is NOT
- Not a love song (the restlessness is about place, not a person)
- Not a full-band arrangement (the sparseness is the point)

# Why This Exists
To capture the feeling of wanting to leave but being stuck.

# Key Decisions

### WHAT: Verse 2 stays in the same location as Verse 1 — no scene change
**WHY:** Early drafts moved the narrator to the train station. It felt like
progress, but the song is about stasis — nothing changes. Keeping the narrator
in bed makes the final line ("I'll go tomorrow") land as self-deception.
**WHEN:** 2026-01-20
**ALTERNATIVES CONSIDERED:**
- Move to train station → rejected: false sense of momentum
- Move to the porch → rejected: still felt like "doing something"

# Open Questions

## To Keep Open
- Whether the narrator is addressing themselves or someone who left
  (both readings add something — resolving it would flatten the lyric)
```

## What BRIEF.md Is Not

- Not a project management tool
- Not a replacement for version control
- Not a rigid schema or database
- Not tied to any vendor or platform

It is intentionally minimal and flexible. A half-finished BRIEF.md is a starting point, not a failure.

## Who Is This For?

- **Creators** (music, writing, design) — preserve the thinking behind your work across sessions and tools
- **Developers** — extend what README and agent.md do with decision records and hierarchical context
- **AI tool users** — give AI a shared reference point instead of re-explaining your project every session
- **Researchers** — study context preservation as an HCI design concern
- **Tool builders** — integrate a portable, open context format

You can use BRIEF.md alone, by hand, without any tool support.

## Why Now?

AI tools create the feeling of shared understanding while having none. Every session resets. Your project's intent shouldn't be locked in an AI's memory, trapped in a SaaS tool, or scattered across Slack messages.

BRIEF.md is the counter-position: context owned by the creator, readable by any tool, portable across time and software.

## Repository Structure

```
spec/                # Core BRIEF.md specification
├── SPECIFICATION.md # The spec (~400 lines) — start here
├── IMPLEMENTATION_GUIDE.md # For tool builders and advanced users
examples/            # Real-world examples by domain
RFCs/                # Proposals and discussion documents
extensions/          # Extension specifications (sonic_arts, narrative_creative, etc.)
```

## Status

This specification is **v1.0-draft** and pre-release. Feedback, critique, and alternative approaches are explicitly welcome.

## Contributing

There is no formal governance yet. If you have feedback, edge cases, critiques, or alternative designs, please open an issue or discussion. This project values clarity over consensus at this stage.

## Author

Created by Gyles Gyesie ([ORCID](https://orcid.org/0009-0008-3394-9243)).

BRIEF.md synthesises architectural decision records, project identity files, and domain-specific metadata into a portable, hierarchical context artifact — and applies it to creative work for the first time.

The concept of *context decay* as applied to project workflows, the BRIEF.md specification, and the supporting theoretical framework were originated by Gyles Gyesie in 2025–2026.

## License

Specification: **CC-BY 4.0**. Implementations may use any license.
