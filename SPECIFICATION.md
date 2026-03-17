# BRIEF.md Specification v2.0

**Author:** Gyles Gyesie
**ORCID:** [0009-0008-3394-9243](https://orcid.org/0009-0008-3394-9243)
**License:** CC-BY 4.0
**DOI:** 10.5281/zenodo.18614773 (White Paper)
**Date:** March 2026

## What Is BRIEF.md?

BRIEF.md is a simple text file that preserves the *why* behind a project: its purpose, boundaries, key decisions, and open questions; in a way that survives pauses, tool changes, collaboration, and time.

Place it at the root of any project folder. No tools required. Readable by humans, parseable by machines.

**Think of it as a project folder from a filing cabinet.** Just as a physical folder contains a card with key project information (title, purpose, status, notes), BRIEF.md is the digital equivalent that tells anyone (or any tool) what they need to know.

**For developers:** Think of BRIEF.md like a README for *intent*, not implementation. Or like an agent.md that also works for humans: it gives AI context on your project's goals, constraints, and past decisions, while also helping your future self and your team.

---

## Quick Start

Create a file called `BRIEF.md` in your project folder with at minimum:

**Music example:**
```markdown
**Project:** Echo Valley
**Type:** song
**Created:** 2026-02-08

# What This Is
A folk song about small-town restlessness told through late-night train imagery.

# Why This Exists
To capture the feeling of wanting to leave but being stuck.

# Key Decisions
(Leave empty until you make a decision worth remembering.)

# Open Questions
(Leave empty until you have unresolved questions.)
```

**Software example:**
```markdown
**Project:** brief-validate
**Type:** cli_tool
**Created:** 2026-03-01

# What This Is
A CLI tool that validates BRIEF.md files against the specification and
reports errors with suggested fixes.

# Why This Exists
Tool builders need a way to verify their BRIEF.md output is spec-compliant.
Currently there's no way to validate without reading the spec manually.

# Key Decisions
(Leave empty until you make a decision worth remembering.)

# Open Questions
(Leave empty until you have unresolved questions.)
```

That's it. You can add more structure later. A half-finished BRIEF.md is not a failure, it's a starting point. The file is useful the moment it has a project name and a sentence about what it is.

---

## Why BRIEF.md Exists

**Tool Fragmentation:** Projects span 5–15 different apps. Context lives in your head or gets lost. BRIEF.md creates a shared context layer that travels with your project.

**AI and Context Decay:** AI tools create the feeling of shared understanding while having none. Most sessions start from zero. BRIEF.md gives both you and your AI tools a concrete reference point.

**Re-entry Cost:** Returning to projects means 15–45 minutes reconstructing what you were thinking. The constraints you were respecting, the alternatives you rejected, the reasons behind your choices. These are the first things to disappear. BRIEF.md turns re-entry into continuation.

### When to Use BRIEF.md

BRIEF.md is most valuable for projects that will be revisited after time, involve multiple tools or collaborators, or where aesthetic and strategic decisions need to be preserved.

You don't need BRIEF.md for every project. Quick experiments, one-off tasks, and throwaway work probably doesn't benefit from it.

---

## File Format

- **Format:** Markdown (`.md`)
- **Encoding:** UTF-8 (full character set including emoji and non-Latin scripts)
- **Line endings:** LF (Unix style)
- **Location:** Project root directory as `BRIEF.md`

---

## File Structure

BRIEF.md can include the following four parts:

```
┌─────────────────────────────────────┐
│ METADATA (required)                 │
│ Project, Type, Created              │
├─────────────────────────────────────┤
│ CORE SECTIONS (required)            │
│ What This Is                        │
│ What This Is NOT                    │
│ Why This Exists                     │
│ Key Decisions                       │
│ Open Questions                      │
├─────────────────────────────────────┤
│ EXTENSIONS (optional)               │
│ e.g. SONIC ARTS, NARRATIVE CREATIVE │
├─────────────────────────────────────┤
│ PROJECT-SPECIFIC (optional)         │
│ Anything unique to this project     │
└─────────────────────────────────────┘
```

---

## Part 1: Metadata (Required)

Every BRIEF.md begins with structured metadata:

```markdown
**Project:** Echo Valley
**Type:** song
**Created:** 2026-01-15
```

### Required Fields

| Field | Description | Example |
|---|---|---|
| **Project** | Name or identifier | `Echo Valley` |
| **Type** | What kind of project (lowercase, community-defined) | `song`, `film`, `feature`, `design` |
| **Created** | When this BRIEF.md was created (YYYY-MM-DD) | `2026-01-15` |

### Optional Fields

| Field | Description | Example |
|---|---|---|
| **Extensions** | Which conceptual frameworks apply | `sonic_arts, narrative_creative` |
| **Status** | Current project state | `concept`, `development`, `production`, `released` |
| **Updated** | Last modified date | `2026-02-01` |
| **Version** | Which spec version this file follows | `2.0` |

### Metadata Format

Fields use the format `**FieldName:** value` (bold markdown). Tools SHOULD be lenient in parsing and also accept common variations such as `FieldName: value` (plain text) and YAML frontmatter.

---

## Part 2: Core Sections (Required)

Five sections that work for any project in any domain. Include at least one; aim for all five as the project develops.

### Internationalisation

Tools SHOULD accept core section headings in common languages. The canonical headings are English, but these aliases are equivalent:

| English (canonical) | German | French | Spanish | Portuguese | Japanese |
|---|---|---|---|---|---|
| What This Is | Was das ist | Qu'est-ce que c'est | Que es esto | O que e isso | これは何か |
| What This Is NOT | Was das nicht ist | Ce que ce n'est pas | Que no es esto | O que nao e | これでないもの |
| Why This Exists | Warum es das gibt | Pourquoi cela existe | Por que existe | Por que existe | なぜ存在するか |
| Key Decisions | Wichtige Entscheidungen | Décisions clés | Decisiones clave | Decisões principais | 主要な決定 |
| Open Questions | Offene Fragen | Questions ouvertes | Preguntas abiertas | Perguntas abertas | 未解決の質問 |

Matching is case-insensitive. Tools that write BRIEF.md files SHOULD output the canonical English headings. Tools that read BRIEF.md files SHOULD accept all aliases.

Additional English aliases are also accepted:
- "Decisions" or "Design Decisions" → Key Decisions
- "Questions" or "Unresolved" → Open Questions

---

### 1. What This Is

Define the project clearly and concisely. 1–3 sentences.

**Music example:**
```markdown
# What This Is
A folk song about small-town restlessness told through late-night train imagery.
Sparse acoustic arrangement with fingerpicked guitar and ambient field recordings.
```

**Software example:**
```markdown
# What This Is
A CLI tool that validates BRIEF.md files against the specification and reports
errors with suggested fixes. Written in Python, distributed via pip.
```

---

### 2. What This Is NOT

Define boundaries and prevent common misunderstandings.

Negative space is often where the most important constraints live. When you return to a project after time, the things you were *avoiding* are the first knowledge to disappear. A producer who returns to a minimalist track might add a synth pad that "sounds nice" but contradicts the original sparse aesthetic. Sometimes that contradiction is exactly the right move; the project has evolved. But when it happens *without you noticing*, it creates a different problem: you end up with conflicting signals in the work, can't tell what the project is trying to be, and the indecisiveness kills momentum. This section doesn't prevent you from crossing your own boundaries. It makes sure you know when you're doing it.

**Music example:**
```markdown
# What This Is NOT
- Not a love song (the restlessness is about place, not a person)
- Not a full-band arrangement (the sparseness is the point)
```

**Software example:**
```markdown
# What This Is NOT
- Not a BRIEF.md editor or IDE plugin (validation only)
- Not a linter for markdown generally (BRIEF.md-specific)
- Not a tool that modifies files (read-only by design)
```

---

### 3. Why This Exists

Document motivation and intended outcome.

**Music example:**
```markdown
# Why This Exists
To capture the specific feeling of lying awake in a small town hearing a train
pass through: wanting to be on it, knowing you won't be. Every production
decision should serve that feeling of suspended restlessness.
```

**Software example:**
```markdown
# Why This Exists
Tool builders need a way to verify their BRIEF.md output is spec-compliant.
Creators need a way to check their files before sharing. Currently there's no
way to validate without reading the spec manually.
```

---

### 4. Key Decisions

Record important decisions with rationale. Use whichever format fits — a sentence or a full record.

**Minimal format** (when the choice is simple):
```markdown
# Key Decisions

### Guitar is slightly out of tune on the recording: keeping it
The imperfection gives the vocal a lonely, unpolished quality that disappears
when the guitar is perfectly in tune. Tried both. The clean version sounds
like a demo; the rough version sounds like a room.
```

**Full format** (when alternatives were carefully weighed):
```markdown
# Key Decisions

### WHAT: Verse 2 stays in the same location as Verse 1: no scene change
**WHY:** Early drafts moved the narrator to the train station in V2. It felt
like progress, but the song is about stasis: the whole point is that nothing
changes. Keeping the narrator in bed, still listening, makes the final line
("I'll go tomorrow") land as self-deception rather than resolve.
**WHEN:** 2026-01-20
**ALTERNATIVES CONSIDERED:**
- Move to train station in V2 → rejected: created false sense of momentum
  that undermined the ending
- Move to the porch (compromise) → rejected: still felt like the narrator
  was "doing something," which breaks the emotional trap
```

Both formats are valid. The minimal format is still a decision record with rationale; it's just less formal. Use the full format when you want to remember the alternatives you rejected.

**Decision relationship fields** (optional, for linking decisions to each other):

| Field | Purpose | Effect on Referenced Decision |
|---|---|---|
| **REPLACES:** | This decision supersedes a prior one | Referenced decision becomes inactive |
| **EXCEPTION TO:** | Creates a scoped exception to an active decision | Referenced decision stays active |
| **AMENDS:** | Modifies an earlier decision without fully replacing it | Both decisions stay active |
| **RESOLVED FROM:** | Links back to an Open Question this decision resolved | Traceability link only |
| **SUPERSEDED BY:** | Passive marker added to the original when replaced | Added automatically or manually |

All relationship fields reference decisions by their title text (the H3 heading or the text after WHAT:).

**Example with relationships:**
```markdown
### WHAT: Switch to room mic blend instead of close-mic only
**WHY:** The close-mic captured detail but lost the natural room decay.
Blending a room mic at ~30% preserves detail while adding spatial depth.
**WHEN:** 2026-02-10
**REPLACES:** Close-mic the guitar exclusively
**ALTERNATIVES CONSIDERED:**
- Keep close-mic only — rejected: too clinical for the aesthetic
- Room mic only — rejected: lost too much pick detail

### WHAT: Use feature flags for gradual rollout
**WHY:** Direct deployment to all users caused a support spike last release.
Feature flags let us validate with 10% of users before full rollout.
**WHEN:** 2026-02-12
**REPLACES:** Deploy directly to all users
**RESOLVED FROM:** How should we handle risky deployments?
```

---

### 5. Open Questions

Track unresolved questions in two categories:

**To Resolve** — decisions pending:
```markdown
## To Resolve
- [ ] The field recording of the actual train bleed: do we keep it or replace
      it with a cleaner sample?
      **Options:** Keep raw recording (has room noise, chair creak) / Replace
      with clean train sample / Layer both
      **Impact:** The raw version is more honest but muddies the low end. The
      clean version sits better in the mix but feels disconnected from the rest
      of the room-recorded aesthetic. This decision affects whether the song
      sounds "captured" or "produced."
```

**To Keep Open** — creative tensions to preserve intentionally:
```markdown
## To Keep Open
- Whether the narrator is addressing themselves or someone who left
  (both readings add something: resolving it would flatten the lyric)
- The ambiguity between literal and metaphorical "train"
  (this dual meaning is the emotional engine of the song)
```

Not every ambiguity needs resolution. In creative work, unresolved tensions can be features, not bugs. This section gives you permission to name what should stay open.

When "To Resolve" questions get answered, move them to Key Decisions with rationale.

---

## Part 3: Extensions (Optional)

Extensions are shared conceptual frameworks that add domain-specific depth to a BRIEF.md file. They are like add-on packs: attach the ones that fit your project.

An instrumental song might use SONIC ARTS. A narrative song might use SONIC ARTS + NARRATIVE CREATIVE. A software project might use STRATEGIC PLANNING + SYSTEM DESIGN. Same extensions, different project types.

### How Extensions Work

Each extension is a markdown section with a level-1 heading, containing subsections for its domain concepts and a references section for influences and sources relevant to that domain.

Fields within extensions can use **free text** (describe things in your own words) or draw from **external ontologies**: standardised vocabularies maintained by domain communities (e.g., MusicBrainz for genre, Theme Ontology for literary themes). Extension specifications recommend which ontologies fit which fields, but free text always works. When projects use the same ontologies, tools can offer smarter autocomplete, search, and cross-project analysis.

```markdown
# SONIC ARTS

## Aesthetic Direction
Sparse, intimate, analog warmth. Think Bon Iver's early recordings
crossed with Adrianne Lenker's fingerpicking.

## Genre
indie-folk, slowcore
<!-- These align with MusicBrainz genre tags, but free text is fine -->

## Production Constraints
- No programmed drums
- Room reverb only (no algorithmic reverb)
- Maximum 8 tracks

## References: Musical
- Bon Iver: For Emma, Forever Ago (production approach)
- Adrianne Lenker: songs/instrumentals (guitar tone)
```

```markdown
# STRATEGIC PLANNING

## Goals
- Reduce onboarding time for new engineers from 2 weeks to 3 days
- Achieve 95% spec compliance across all generated BRIEF.md files

## Constraints
- Must work offline (no cloud dependencies)
- Budget: one engineer for 6 weeks
- Cannot break existing CI/CD pipelines

## Assumptions
- Teams already use markdown for documentation
- Git is the primary version control system

## References: Strategic
- Shape Up (Basecamp) — appetite-based scoping
- Team Topologies — cognitive load as a design constraint
```

### Defining Extensions

No extensions have been formally published yet. The examples above (SONIC ARTS, STRATEGIC PLANNING, etc.) illustrate the pattern — they are not stable specifications.

Anyone can propose a new extension. Proposals are welcome as GitHub issues. Each extension specification should document its name, purpose, required and optional subsections, and any recommended ontologies.

---

## Part 4: Project-Specific (Optional)

Content that doesn't fit in the core sections or existing extensions.

```markdown
# SONGWRITING SPECIFIC

## Vocal Approach
Conversational delivery, not performative. Think talking to someone
in the next room, not singing to an audience.
```

```markdown
# DEPLOYMENT SPECIFIC

## Rollout Strategy
Canary deployment to 5% of traffic, then gradual ramp over 48 hours.
Automatic rollback if error rate exceeds 0.5%.
```

If project-specific content becomes common across multiple projects, consider proposing it as a new extension.

---

## Recording External Tool Sessions

When work in an external tool (Figma, a DAW, a design tool, etc.) produces decisions or artefacts worth preserving, record a breadcrumb in a dedicated section:

```markdown
# External Tool Sessions
- 2026-02-14 Figma: 1 decision captured — Album cover uses train silhouette
- 2026-02-16 Miro: 1 decision captured — Architecture uses event-driven pattern
```

Each line records the date, tool name, and a summary of what was captured. Associated decisions are recorded in the Key Decisions section with the **WHEN** field matching the session date, giving tools a way to correlate sessions with their decisions.

---

## Folder Structure and Hierarchy

### Core Principle

**Folder nesting = context nesting.** No explicit links needed.

Tools that implement hierarchy support walk up the directory tree, reading any parent BRIEF.md files they find. Context accumulates from each level. This behaviour is opt-in at the tool level — not all tools are required to implement it.

```
the-wanderers/                  # Artist
├── BRIEF.md                    # "sparse, intimate production"
└── albums/
    └── midnight-train/         # Album
        ├── BRIEF.md            # "nighttime restlessness theme"
        └── songs/
            └── echo-valley/    # Song
                └── BRIEF.md    # "small-town restlessness, 85 BPM"
```

A tool opening `echo-valley/BRIEF.md` discovers:
1. Song context: small-town restlessness, 85 BPM
2. Album context: nighttime theme, slow tempo
3. Artist context: sparse, intimate production

The tool now knows the song's intent, the album's theme, *and* the artist's style: without anyone configuring relationships.

### How Deep?

Use whatever depth makes sense for your domain:

- 2 levels: Designer → Design
- 3 levels: Artist → Album → Song
- 4 levels: Studio → Franchise → Film → Scene
- 5+ levels: Company → Product → Module → Feature → Component

### Context Is Advisory

Parent context is advisory, not prescriptive. Children make their own decisions. Tools may warn about conflicts but don't enforce. Users make final choices.

### Walking Up vs Walking Down

The core context-discovery mechanism is **walking up**: when you open a file, you discover the bigger picture around it. But tools can also **walk down** the hierarchy to scan what exists within a collection: finding which songs share a theme, spotting patterns across a body of work, or giving an overview of what a project contains. Walking down is a separate operation: more of an intentional "show me the landscape" action than something that happens automatically on every file open.

---

## Design Principles

1. **Simplicity First.** Minimal required structure. Complexity is opt-in.
2. **Human-Readable First.** Markdown, not JSON. Prose, not rigid schemas.
3. **Power Through Composition.** Core handles basics. Extensions add depth.
4. **Folder-Based Relationships.** No links to maintain. Structure is self-evident.
5. **Bottom-Up.** Projects define themselves first. Collections observe patterns.
6. **Open and Portable.** No vendor control. CC-BY 4.0 license.

---

## Non-Goals

BRIEF.md does **not** aim to:

- Replace project management systems (Jira, Asana, Notion)
- Enforce workflows, methodologies, or creative processes
- Act as a task tracker, roadmap, or planning document
- Standardize creativity, taste, or decision-making
- Serve as a database, knowledge graph, or analytics schema
- Require automation, AI, or tool integration to be valuable

If BRIEF.md is becoming heavy, bureaucratic, or prescriptive, it is being misused. The file should remain readable in under 5 minutes, editable by hand, and useful even if no tool ever parses it.

---

## Validation

A **valid** BRIEF.md file:
- Is valid UTF-8 encoded Markdown
- Contains the three required metadata fields (Project, Type, Created)
- Contains at least one core section

A **well-formed** BRIEF.md file additionally:
- Includes all five core sections
- Uses consistent heading levels
- Follows the four-part structure (metadata → core → extensions → specific)

The spec emphasises flexibility over rigidity. Tools can validate more strictly if needed. Users have final control.

---

## Complete Example

```markdown
**Project:** Echo Valley
**Type:** song
**Extensions:** sonic_arts, narrative_creative
**Status:** development
**Created:** 2026-01-15
**Updated:** 2026-02-01

# What This Is
A folk song about small-town restlessness told through late-night train imagery.
Sparse acoustic arrangement with fingerpicked guitar and ambient field recordings.

# What This Is NOT
- Not a love song (the restlessness is about place, not a person)
- Not a full-band arrangement (the sparseness is the point)

# Why This Exists
To capture the specific feeling of lying awake in a small town hearing a train
pass through: wanting to be on it, knowing you won't be.

# Key Decisions

### WHAT: Verse 2 stays in the same location as Verse 1: no scene change
**WHY:** Early drafts moved the narrator to the train station in V2. It felt
like progress, but the song is about stasis: the whole point is that nothing
changes. Keeping the narrator in bed, still listening, makes the final line
("I'll go tomorrow") land as self-deception rather than resolve.
**WHEN:** 2026-01-20
**ALTERNATIVES CONSIDERED:**
- Move to train station in V2 → rejected: created false sense of momentum
  that undermined the ending
- Move to the porch (compromise) → rejected: still felt like the narrator
  was "doing something," which breaks the emotional trap

### Guitar is slightly out of tune on the recording: keeping it
The imperfection gives the vocal a lonely, unpolished quality that disappears
when the guitar is perfectly in tune. Tried both. The clean version sounds
like a demo; the rough version sounds like a room.

# Open Questions

## To Resolve
- [ ] The field recording of the actual train bleed: do we keep it or replace
      it with a cleaner sample?
      **Options:** Keep raw recording (has room noise, chair creak) / Replace
      with clean train sample / Layer both
      **Impact:** The raw version is more honest but muddies the low end. The
      clean version sits better in the mix but feels disconnected from the rest
      of the room-recorded aesthetic.

## To Keep Open
- Whether the narrator is addressing themselves or someone who left
  (both readings add something: resolving it would flatten the lyric)

# SONIC ARTS

## Aesthetic Direction
Sparse, intimate, analog warmth. Room recordings over studio polish.

## Production Constraints
- No programmed drums
- Room reverb only
- Maximum 8 tracks

## References: Musical
- Bon Iver: For Emma, Forever Ago (production approach)
- Adrianne Lenker: songs/instrumentals (guitar tone)

# NARRATIVE CREATIVE

## Core Themes
- Restlessness without resolution
- The tension between belonging and wanting to leave
- Night as a liminal space

## Narrative Perspective
First person, present tense. The narrator is in the moment, not reflecting.

## References: Literary
- Raymond Carver — minimalist observation style
- Jason Isbell — small-town specificity
```

### Complete Example: Software Project

```markdown
**Project:** brief-validate
**Type:** cli_tool
**Extensions:** system_design
**Status:** development
**Created:** 2026-02-20
**Updated:** 2026-03-10

# What This Is
A CLI tool that validates BRIEF.md files against the specification and reports
errors with suggested fixes. Written in Python, distributed via pip.

# What This Is NOT
- Not a BRIEF.md editor or IDE plugin (validation only)
- Not a linter for markdown generally (BRIEF.md-specific rules only)
- Not a tool that modifies files (read-only by design)

# Why This Exists
Tool builders need a way to verify their BRIEF.md output is spec-compliant.
Creators need a way to check their files before sharing. Currently there's no
way to validate without reading the spec manually.

# Key Decisions

### WHAT: Use exit codes to indicate validation severity
**WHY:** CI pipelines need a machine-readable signal. Exit 0 = valid,
exit 1 = errors found, exit 2 = warnings only. This lets teams gate
merges on BRIEF.md validity without custom scripting.
**WHEN:** 2026-02-25
**ALTERNATIVES CONSIDERED:**
- JSON output only — rejected: requires parsing, not pipeline-friendly
- Always exit 0 with report — rejected: no way to fail a build

### Validate structure only, not prose quality
Prose quality is subjective and domain-specific. The tool checks that
required sections exist and metadata is well-formed, not whether the
content is useful.

# Open Questions

## To Resolve
- [ ] Should the tool auto-detect spec version from the file, or require
      it as a flag?
      **Options:** Auto-detect from Version field / Require --spec-version
      flag / Default to latest, allow override
      **Impact:** Auto-detect is friendlier but may guess wrong on files
      missing the Version field.

## To Keep Open
- Whether validation should eventually cover extension-specific rules
  (this depends on how extensions evolve and may not have one answer)

# SYSTEM DESIGN

## Architecture
Single-pass parser with pluggable rule engine. Rules are pure functions
that receive parsed BRIEF.md and return diagnostics.

## Technical Constraints
- Python 3.9+ (no external dependencies for core validation)
- Must handle files up to 10,000 lines without noticeable delay
- UTF-8 only (per spec requirement)

## References: Technical
- ruff — fast Python linter (architecture inspiration)
- markdownlint — rule-based markdown validation approach
```

---

## License

This specification is released under **CC-BY 4.0**. Implementations may use any license.

---

## Status and Contributing

This specification is **v2.0** (March 2026).

Governance processes will be established as the community grows. For now, proposals and feedback are welcome as GitHub issues or discussions.

For tool implementation details, interoperability guidelines, and advanced topics, see the [Implementation Guide](IMPLEMENTATION_GUIDE.md).
