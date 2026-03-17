# BRIEF.md Implementation Guide

**Author:** Gyles Gyesie
**License:** CC-BY 4.0

This guide is for tool builders, advanced users, and anyone integrating BRIEF.md into software. For the core specification (what BRIEF.md is and how to write one), see [SPECIFICATION.md](SPECIFICATION.md).

---

## Table of Contents

- [Technical Specifications](#technical-specifications)
- [Context Discovery](#context-discovery)
- [Context Application](#context-application)
- [Interoperability Guidelines](#interoperability-guidelines)
- [Extensions: Detailed Architecture](#extensions-detailed-architecture)
- [Project Types vs Extensions](#project-types-vs-extensions)
- [Best Practices](#best-practices)
- [Security Considerations](#security-considerations)
- [Metadata Lifecycle and Public Exposure](#metadata-lifecycle-and-public-exposure)
- [Versioning](#versioning)
- [Ontology Pack Architecture (Planned)](#ontology-pack-architecture-planned)
- [Type Guide System (Planned)](#type-guide-system-planned)
- [Project Maturity Assessment](#project-maturity-assessment)
- [Configuration](#configuration)
- [Building a BRIEF.md Integration](#building-a-briefmd-integration)

---

## Technical Specifications

### Metadata Field Format

Metadata fields follow a canonical format for tool interoperability:

```markdown
**FieldName:** value
```

**Canonical rules:**
- Field names in Title Case (e.g., `Project`, `Type`, `Created`)
- Bold formatting: `**FieldName:**` (two asterisks each side)
- Colon immediately after field name, followed by single space
- One field per line

**Lenient parsing (recommended):**

Tools SHOULD accept common variations to prioritise human-readability:

| Format | Status |
|---|---|
| `**Project:** Echo Valley` | Canonical |
| `Project: Echo Valley` | Accept (plain text) |
| `**Project :** Echo Valley` | Accept (extra space before colon) |
| `**project:** Echo Valley` | Accept (case-insensitive matching) |
| YAML frontmatter | Accept (alternative format) |

Tools that write BRIEF.md files SHOULD output the canonical format. Tools that read BRIEF.md files SHOULD be lenient.

### Naming Conventions

**Project Type values:**
- Always lowercase
- Underscores for multi-word types: `song`, `album`, `product_line`

**Extension names:**
- In markdown headings: ALL CAPS with spaces: `# SONIC ARTS`
- In metadata field: lowercase with underscores, comma-separated: `**Extensions:** sonic_arts, narrative_creative`

### Section Heading Aliases

Tools SHOULD accept core section headings in multiple languages and common English variations. See the Internationalisation section in [SPECIFICATION.md](SPECIFICATION.md) for the full alias table. Matching is case-insensitive. Tools that write BRIEF.md files SHOULD output the canonical English headings.

### Character Encoding

All text content supports the full UTF-8 character set including emoji, non-Latin scripts, and special characters. Tools MUST handle these correctly in both parsing and display.

### Alternative Metadata Formats

Tools MAY support additional metadata representations (YAML frontmatter, JSON, etc.) but MUST support the standard markdown field format. Example YAML equivalent:

```yaml
---
project: Echo Valley
type: song
created: 2026-01-15
extensions: [sonic_arts, narrative_creative]
status: development
---
```

---

## Context Discovery

### The Hierarchy Walk

When a tool opens a BRIEF.md file, it should discover parent context by walking up the directory tree:

```
function discover_context(brief_path):
    context_stack = []
    current_path = brief_path

    while current_path exists:
        if BRIEF.md exists at current_path:
            context_stack.append(parse_brief(current_path))
        current_path = parent_directory(current_path)

    return context_stack  # [item, collection, entity, ...]
```

**Depth limit:** Tools SHOULD implement a practical depth limit (recommended: 10 levels) and MUST handle filesystem loops gracefully (symlinks, mount points).

**Stop conditions:** Tools SHOULD stop walking when they reach a filesystem root, a version control root (`.git` directory), or the configured depth limit.

### Example: Music Hierarchy

```
the-wanderers/
├── BRIEF.md                    # Artist aesthetic: "sparse, intimate"
└── albums/
    └── midnight-train/
        ├── BRIEF.md            # Album theme: "nighttime restlessness"
        └── songs/
            └── echo-valley/
                └── BRIEF.md    # Song intent: "unresolved tension, 85 BPM"
```

Tool opens `echo-valley/BRIEF.md` and accumulates:

1. **Song:** small-town restlessness, 85 BPM
2. **Album:** nighttime theme, slow tempo
3. **Artist:** sparse, intimate production

### Example: Software Hierarchy

```
acme-corp/
├── BRIEF.md                    # Company: "developer-first, open-source ethos"
└── products/
    └── dataflow/
        ├── BRIEF.md            # Product: "real-time data pipeline framework"
        └── features/
            └── retry-engine/
                └── BRIEF.md    # Feature: "configurable retry with backoff"
```

### No Inheritance: Advisory Context

Context from parent BRIEF.md files is **advisory, not prescriptive**:

- Parents describe patterns they observe
- Children make their own decisions
- Tools warn about conflicts but don't enforce
- Users make final choices

#### Conflict Detection

The reference implementation uses a two-layer conflict detection approach:

**Layer 1: Heuristic** — Compare decision-to-decision, decision-to-constraint, and constraint-to-constraint pairs within a single BRIEF.md and across the parent/child hierarchy. Resolution options: `supersede` (replace the old decision), `exception` (scoped override), `update` (amend the original), or `dismiss` (acknowledge and move on).

**Layer 2: Semantic (optional)** — When AI capabilities are available, decision pairs can be analysed for semantic contradiction. Use a confidence threshold (recommended: 0.6 minimum) to avoid false positives. Deduplicate against heuristic results.

**Conflict suppression via intentional tensions:** When a project type has known creative or technical tensions (e.g., "authenticity vs. polish" in music, "speed vs. quality" in software), the conflict checker should treat matching decision pairs as intentional trade-offs rather than contradictions.

Conflict detection remains optional — tools at Level 1 or Level 2 integration are not expected to implement it.

### Walking Down: Collection Discovery

The up-walk is the core mechanism: discover the bigger context around the file you've opened. But tools can also **walk down** the hierarchy: scanning child directories for BRIEF.md files to show what a collection contains.

Use cases for walking down:
- An artist-level view that shows themes across all albums and songs
- A product-level view that surfaces which features have open questions
- Pattern detection: which projects share constraints, references, or extensions

Implementation notes:
- Walking down is an intentional user action (e.g., "show me what's in this collection"), not something that triggers on every file open
- Down-walks can be expensive in large hierarchies: consider lazy loading or depth limits
- Tools SHOULD handle directories without BRIEF.md files gracefully (skip, don't error)

---

## Context Application

Tools use discovered context in many ways. Here are illustrative patterns:

### Parent-Aware Operations

- **Alignment checking:** Does this item conflict with parent constraints?
- **Default suggestion:** What values are typical based on parent context?
- **Decision validation:** Does this choice contradict parent decisions?
- **Contextual intelligence:** What does parent context tell us about this item?

### Standalone Operations

- **Autocomplete:** Suggest field values from domain ontologies
- **Search and discovery:** Find projects by type, extension, or content
- **Visualisation:** Map relationships between projects
- **Export:** Convert to other formats (JSON, YAML, etc.)
- **Analytics:** Track decision patterns across projects
- **History:** Show evolution of decisions over time
- **Validation:** Check against extension requirements

### Example: DAW Integration

When a DAW opens a project with BRIEF.md:

1. Read the song's BRIEF.md: emotional intent, tempo, constraints
2. Walk up to album BRIEF.md: thematic context, genre
3. Walk up to artist BRIEF.md: production aesthetic

The DAW can then:
- Suggest presets aligned with the aesthetic
- Warn when changes contradict constraints
- Frame AI suggestions in emotional terms, not just technical ones

### Example: IDE Integration

When an IDE opens a feature directory with BRIEF.md:

1. Read the feature's BRIEF.md: what it does, constraints, decisions
2. Walk up to product BRIEF.md: architectural principles, tech stack
3. Walk up to company BRIEF.md: engineering philosophy

The IDE can then:
- Show relevant decisions when editing related code
- Warn when a change conflicts with documented constraints
- Provide decision context to AI coding assistants

---

## Interoperability Guidelines

### Handling Unknown Content

**Unknown metadata fields:**
- Tools MUST ignore gracefully
- Tools SHOULD preserve when editing
- Tools MAY log warnings

**Unknown extensions:**
- Tools MUST ignore sections they don't understand
- Tools MUST preserve content when editing files
- Tools MAY warn users about unrecognised extensions

**Unknown project types:**
- Tools SHOULD treat as generic project
- Tools MAY provide reduced functionality
- Tools MUST NOT reject the file

### Handling Malformed Content

**Invalid required fields:**
- Tools SHOULD provide clear error messages
- Example: Missing `**Project:**` field

**Partial files:**
- Tools MAY accept files missing optional sections
- Tools SHOULD warn about missing recommended sections

### Version Compatibility

**Forward compatibility (reading newer versions):**
- Tools reading v2.0 SHOULD accept v2.x files
- Tools SHOULD ignore new fields added in minor versions
- Tools SHOULD warn on major version mismatches

**Backward compatibility (reading older versions):**
- Tools implementing v2.0 SHOULD support v1.0 files
- Tools MUST NOT require fields added in v2.0 (e.g., decision relationship fields)
- v1.0 files are valid v2.0 files — no migration needed

**Version indicators:**
- Files MAY include `**Version:** 2.0` in metadata
- If absent, assume latest v2.x

### Preserving User Content

When editing BRIEF.md files, tools MUST:
- Preserve all sections they don't understand
- Maintain original formatting where possible
- Not delete unknown fields or content
- Add clear markers when adding tool-specific content

### Tool-Specific Sections

Tools may add custom content using a dedicated section at the end of the file:

```markdown
# TOOL SPECIFIC: SongMuse

Last session: 2026-01-20
Tool version: 2.1.3
```

**Guidelines:**
- Use all caps `TOOL SPECIFIC` prefix
- Include tool name
- Keep at end of file
- Other tools MUST preserve this section

### Comments

Standard HTML comments are supported:

```markdown
<!-- This is a comment that tools should preserve but not display -->
```

Tools SHOULD preserve comments when editing and not render them in UI.

---

## Extensions: Detailed Architecture

### What Extensions Are

An extension (called "Domain Families" in some earlier documentation) is:
- A named collection of related concepts
- Defined by communities
- Reusable across different project types
- Documented in a separate extension specification

### Structure of an Extension

Each extension is a markdown section with:

```markdown
# [EXTENSION NAME]

## [Subsection 1]
[Fields and content defined by extension spec]

## [Subsection 2]
[Fields and content defined by extension spec]

## References: [Type]
[References relevant to this extension]
```

### External Ontologies and Taxonomies

Extension specifications may recommend external ontologies for standardised vocabularies:

- **SONIC ARTS** might recommend MusicBrainz for genre classification
- **NARRATIVE CREATIVE** might recommend Theme Ontology for literary themes
- **SYSTEM DESIGN** might recommend standard architecture pattern catalogues

These are recommendations, not requirements. Users can use free text if ontologies don't fit their needs. When multiple projects follow the same ontologies, tools can provide smarter autocomplete, validation, semantic search, and cross-project analysis.

### Defining New Extensions

Anyone can propose a new extension. For now, proposals are welcome as GitHub issues. Each extension spec should document:

- Name and purpose
- Required and optional subsections
- Field names and their semantics
- Recommended ontologies (if any)
- How tools should interpret the data

### Extension Status

No extensions have been formally published yet. The names used in examples throughout this guide (SONIC ARTS, STRATEGIC PLANNING, SYSTEM DESIGN, etc.) illustrate the extension pattern but are not stable specifications. See the core specification for how to propose new extensions.

---

## Project Types vs Extensions

These two metadata fields serve different purposes and are independent of each other.

**Type** = what kind of project this IS.
- Community-defined (each domain defines their own)
- Examples: `song`, `album`, `film`, `feature`, `design`
- Helps tools understand the project's role in a hierarchy

**Extensions** = conceptual frameworks this project USES.
- Cross-cutting (same extension used by multiple types)
- Examples: SONIC ARTS, NARRATIVE CREATIVE
- Optional: use what fits

**Examples of flexibility:**

| Project | Type | Extensions |
|---|---|---|
| Instrumental song | `song` | SONIC ARTS |
| Narrative song | `song` | SONIC ARTS + NARRATIVE CREATIVE |
| Documentary film | `film` | VISUAL STORYTELLING |
| Narrative film | `film` | VISUAL STORYTELLING + NARRATIVE CREATIVE |
| Technical product | `product` | STRATEGIC PLANNING + SYSTEM DESIGN |
| Creative product | `product` | STRATEGIC PLANNING + NARRATIVE CREATIVE |

**Analogy:** Type is your profession (engineer, filmmaker). Extensions are skill sets you use (data analysis, visual design, storytelling). Same skills, different professions.

---

## Best Practices

### File Size and Scope

**Keep BRIEF.md focused on context, not content:**
- Typical size: 100–500 lines
- If exceeding 1,000 lines, consider splitting
- Don't paste entire scripts, specifications, or code
- Link to detailed documents rather than embedding

**What belongs in BRIEF.md:** Project identity and purpose, key decisions with rationale, constraints and boundaries, references and influences.

**What doesn't belong:** Full scripts or specifications, detailed implementation code, extensive research notes, task lists or timelines, marketing copy.

### When to Split

Consider multiple BRIEF.md files when:
- Project has clear subcomponents
- Different teams work on different parts
- Sections exceed 500 lines
- Contexts are truly independent

```
film/BRIEF.md              # Overall film context
film/scenes/s01/BRIEF.md   # Scene-specific context
film/scenes/s02/BRIEF.md   # Scene-specific context
```

### Solo vs Team Use

The format is the same; the dynamics differ:

**Solo creators:** BRIEF.md is mainly communication with your future self. Focus on the sections most likely to fade: Key Decisions, What This Is NOT, and motivational context (Why This Exists). You'll remember *what* the project is; you won't remember *why* you made specific choices.

**Teams:** BRIEF.md is alignment infrastructure. Focus on Key Decisions (prevents re-litigating resolved debates), constraints (prevents conflicting changes), and Open Questions (makes uncertainty visible). New team members read BRIEF.md to understand "why we built it this way" without archaeological digging.

### Updating BRIEF.md

**When to update:**
- When making significant decisions
- When constraints change
- When resolving open questions
- When project direction shifts

**How to update:**

For decisions with alternatives, document in Key Decisions first, then update affected sections. This creates a clear cause-and-effect relationship:

```markdown
# Key Decisions

### WHAT: Expanded scope to include acceptance theme
**WHY:** Bridge felt incomplete without emotional resolution
**WHEN:** 2026-02-03
**ALTERNATIVES CONSIDERED:**
- Keep pure restlessness → rejected: too one-dimensional
- Add anger/frustration → rejected: wrong emotional tone

# What This Is
A song about small-town restlessness and the quiet acceptance that follows.

**Updated:** 2026-02-03
```

For evolutionary understanding (no alternatives existed), update sections directly.

For resolved Open Questions, move them to Key Decisions with rationale.

**When to use Open Questions vs Key Decisions directly:**

If a question needs time to resolve (days or weeks of deliberation), log it as an Open Question first. If it's resolved immediately in the same work session, skip directly to Key Decisions.

### Version Control

- Commit BRIEF.md alongside related project changes
- Use meaningful commit messages
- Git provides complete change history: no need for separate versioning in the file
- Treat BRIEF.md changes as significant events

### Multi-Author Projects

- Establish ownership/approval process for BRIEF.md changes
- Review BRIEF.md updates as a team
- Resolve conflicts in Open Questions before moving to Key Decisions

### Privacy and Sensitivity

Before sharing BRIEF.md externally:
- Review for confidential information
- Consider if decisions reveal unreleased plans
- Remove sensitive business details
- Remember BRIEF.md files may be indexed or cached

---

## Security Considerations

BRIEF.md files may contain sensitive information (unreleased project details, strategic decisions, proprietary technical choices). Security is an implementation concern, not a format specification.

**General guidance:** Apply security controls appropriate to the sensitivity of your content. This includes file system permissions, encryption, access control, and compliance requirements as appropriate for your organisation.

**Security postures range from:**
- **Open source projects:** Public repository, community access
- **Small teams:** Private repository, team-level access
- **Enterprise:** On-premise infrastructure, strict access controls, audit logging, compliance requirements

The specification intentionally does not mandate specific security measures, allowing organisations to apply appropriate controls based on their needs.

---

## Metadata Lifecycle and Public Exposure

BRIEF.md files are typically private during development. After release, certain metadata may become valuable for public discovery.

### What Might Be Public After Release

**Potentially shareable:** Themes, genres, moods, instrumentation, production techniques, architecture patterns, technology choices: metadata that helps discovery.

**Typically private:** Key Decisions (reveals strategic reasoning), Open Questions (shows internal process), "What This Is NOT" (may reveal competitive positioning).

These are considerations, not rules. Your context determines what to share.

### Tool Responsibilities for Export

Tools that export metadata to public platforms SHOULD:

1. **Allow user review**: show exactly what will be made public
2. **Require explicit approval** before export
3. **Provide selection interface**: allow users to include/exclude specific fields
4. **Provide sensible defaults**: typically excluding Key Decisions and Open Questions
5. **Map to platform formats**: convert BRIEF.md metadata to platform-specific taxonomies
6. **Respect user control**: support "keep everything private" as a valid choice

**Privacy principle:** Users retain full control over what becomes public. The default is private. Public exposure requires explicit user action.

---

## Versioning

### Specification Versioning

This specification uses semantic versioning:
- **Major:** Breaking changes to core structure
- **Minor:** Additive changes (new extensions, new fields)
- **Patch:** Clarifications and fixes

### File Versioning

Individual BRIEF.md files may include a `**Version:**` field indicating which spec version they follow. If omitted, tools assume latest v2.x.

---

## Ontology Pack Architecture (Planned)

> **Status:** This architecture is implemented in the MCP reference server. No ontology packs have been formally published yet. The format may evolve before stable release.

### What Ontology Packs Are

The spec mentions that extension fields can draw from external ontologies. The reference implementation provides a concrete pack system for distributing, installing, and querying these ontologies.

A pack is a JSON file containing a named collection of entries with searchable metadata. Packs are stored at `~/.brief/ontologies/{packName}/pack.json`.

### Pack Format

```json
{
  "name": "theme-pack",
  "version": "1.0.0",
  "description": "Literary and narrative themes",
  "searchFields": ["label", "keywords", "aliases"],
  "synonyms": { "longing": ["yearning", "desire", "pining"] },
  "entries": [
    {
      "id": "nostalgia",
      "label": "Nostalgia",
      "description": "A sentimental longing for the past",
      "keywords": ["memory", "past", "longing"],
      "aliases": ["nostalgic"],
      "categories": ["emotion", "temporal"],
      "tags": ["reflective"],
      "parentId": null,
      "references": []
    }
  ]
}
```

**Pack-level fields:** `name` (required), `version`, `description`, `searchFields` (which entry fields to index — default: label, keywords, aliases), `synonyms` (pack-level synonym map for search expansion).

**Entry fields:** `id` (required), `label` (required), `description`, `keywords`, `aliases`, `categories`, `tags`, `parentId` (for tree structures), `references` (array of source objects).

### Ontology Tagging

Tools can tag BRIEF.md content with ontology entries using HTML comments:

```markdown
## Core Themes
<!-- brief:ontology theme-pack nostalgia "Nostalgia" -->
- Restlessness without resolution
```

Format: `<!-- brief:ontology {packName} {entryId} "{label}" -->`. Tags are advisory annotations — they enable cross-project search but do not change the meaning of the content.

### Memory Management

- Use LRU eviction for loaded pack indexes (recommended budget: 100 MB)
- Staleness detection: reload if on-disk file is newer than cached version (check interval: 60s)
- Maximum individual pack size: 50 MB

---

## Type Guide System (Planned)

> **Status:** Type guides are implemented in the MCP reference server but no guides have been formally published. The format is subject to change.

### What Type Guides Are

Type guides are domain-specific templates that help tools bootstrap BRIEF.md files for particular project types. When a user creates a `song` project, a type guide for `song` can suggest relevant extensions, ontologies, known creative tensions, and quality signals.

Type guides are markdown files with YAML frontmatter stored at `~/.brief/type-guides/{type}.md`.

### Type Guide Format

```yaml
---
type: song
bootstrapping: true
source: bundled
version: "1.0"
conflict_patterns:
  - ["emotion", "clarity"]
  - ["simplicity", "depth"]
suggested_extensions:
  - name: SONIC ARTS
    subsections:
      - name: Sound Palette
        mode: ontology
        ontology: music-theory
suggested_ontologies:
  - name: music-theory
    description: "Scales, modes, harmonic concepts"
---
```

The markdown body contains guidance organised into universal dimensions.

### Universal Dimensions

All type guides share these structural categories:

| Dimension | Purpose |
|---|---|
| Medium & Discipline | What form the work takes |
| Primary Activities | What creative/technical work is involved |
| Outputs & Deliverables | What the project produces |
| Audience & Expectations | Who it's for and what they expect |
| Success Criteria | How to evaluate the result |
| Known Tensions | Common creative/technical trade-offs for this type |
| Quality Signals | Indicators that the work is succeeding |

### Conflict Patterns

The `conflict_patterns` field lists common tensions for the project type. These serve two purposes: surfacing trade-offs early during onboarding, and suppressing false positives in conflict detection when opposing decisions match a known tension.

### Source Tracking

| Source | Meaning |
|---|---|
| `bundled` | Ships with the tool |
| `ai_generated` | Created by AI during a session |
| `community` | From a shared community repository |
| `user_edited` | Modified by the user |

---

## Project Maturity Assessment

Tools can assess a project's lifecycle phase by analysing the structure and density of its BRIEF.md file. This helps tools calibrate their guidance.

### Maturity Levels

| Level | Decision Count | Characteristics |
|---|---|---|
| **Nascent** | 0–2 | Project just starting. Many sections empty. Focus on capturing initial intent. |
| **Developing** | 3–5 | Core identity established. Decisions accumulating. Good time to suggest format upgrades. |
| **Maturing** | 6–10 | Rich decision history. Relationships between decisions emerging. Conflict checking becomes valuable. |
| **Established** | 11+ | Comprehensive record. Focus shifts to maintenance and superseding outdated decisions. |

### Signals Tracked

- **Section fill-state:** which core sections have content vs. are empty
- **Decision format:** count of minimal-format vs. full-format decisions
- **Upgradeable decisions:** minimal decisions missing WHAT, WHY, WHEN, or ALTERNATIVES fields
- **Open question count:** unresolved questions (both to-resolve and to-keep-open)
- **Superseded decision count:** how many decisions have been replaced over time

Tools use maturity signals to tailor prompts: e.g., in a "developing" project, suggest upgrading minimal decisions to full format with rationale.

---

## Configuration

### Directory Layout

The reference implementation uses `~/.brief/` as the configuration root:

```
~/.brief/
  config.json              # User configuration
  ontologies/              # Installed ontology packs
    theme-pack/
      pack.json
  type-guides/             # Type guide files
    _generic.md            # Fallback bootstrapping guide
    song.md
```

### Key Configuration Settings

| Setting | Default | Description |
|---|---|---|
| `workspaces` | `["~/projects"]` | Directories to scan for BRIEF.md projects |
| `hierarchy_depth_limit` | 10 | Max levels to walk up when discovering context |
| `context_size_limit` | 50 KB | Max accumulated context size from hierarchy walk |
| `project_scan_depth` | 5 | Max depth when scanning for projects within a workspace |
| `operation_timeout` | 30s | Timeout for file operations |
| `max_pack_size` | 50 MB | Maximum size for a single ontology pack |
| `index_memory_budget` | 100 MB | Memory limit for all loaded ontology indexes |
| `write_lock_timeout` | 10s | Timeout for atomic write locks |
| `index_staleness_period` | 60s | How often to check if cached pack data is stale |

On Unix systems, `config.json` should be created with mode `0600` and subdirectories with `0700`.

---

## Building a BRIEF.md Integration

If you want to add BRIEF.md support to your tool, here's a minimal integration path:

### Level 1: Read and Display
- Parse metadata fields (lenient parsing)
- Display core sections to users
- No hierarchy walk needed

### Level 2: Hierarchy-Aware
- Walk up directory tree to discover parent BRIEF.md files
- Display accumulated context
- Respect the 10-level depth limit

### Level 3: Write Support
- Generate BRIEF.md skeleton on project creation
- Prompt users for context at natural decision points
- Preserve unknown sections and comments when editing

### Level 4: AI Integration
- Feed BRIEF.md content to AI tools at session start
- Propose BRIEF.md updates after working sessions
- Check AI output against documented constraints

#### Session Re-entry

When an AI tool starts a new conversation about an existing project, it should perform a re-entry sequence rather than starting from scratch:

1. Load the full BRIEF.md content for the active project
2. Walk up the hierarchy for parent context
3. Surface open questions (especially "To Resolve") as natural conversation starters
4. Report maturity signals so guidance is calibrated to the project's lifecycle phase
5. Note any external sessions captured since the last conversation

This turns session start from "what are we working on?" into "here's where we left off."

#### Interaction Patterns

AI tools working with BRIEF.md projects should implement these patterns:

| # | Pattern | Description |
|---|---|---|
| 1 | Session Start | Load full project state, open questions, and maturity signals at conversation start |
| 2 | Decision Capture | Recognise when users make decisions in conversation and offer to record them |
| 3 | Question Surfacing | Identify unresolved questions during work and prompt the user to log them |
| 4 | External Session | After discussing work done in another tool, offer to capture it |
| 5 | Conflict Resolution | When adding decisions that may conflict with existing ones, surface the conflict |
| 6 | Extension Setup | When project context suggests a domain framework would help, suggest relevant extensions |
| 7 | Ontology Exploration | When users need vocabulary for a domain, guide them through ontology browsing |
| 8 | Collaborative Authoring | When writing sections, ask the user first, draft based on input, then refine |
| 9 | Type Guide Review | At project creation, surface the type guide and walk through its dimensions |
| 10 | Type Guide Creation | When no guide exists for a project type, offer to create one |

#### Decision Recognition

AI tools should watch for commitment language ("I'll go with X", "let's use X", "decided to...") and offer to record decisions. Key principles:

- **Confirm before recording** — if it's unclear whether something is a decision or thinking aloud, ask
- **Avoid over-logging** — only record choices that affect project direction, constrain future work, or would be non-obvious to someone joining later
- **Use appropriate format** — full format (WHAT/WHY/WHEN/ALTERNATIVES) when alternatives were weighed; minimal format when the choice was straightforward
- **Respect deliberation** — if the user is still deliberating, log as an Open Question, not a decision
- **Capture retroactively** — if a past decision surfaces in conversation, offer to backfill it

#### Question Surfacing

AI tools should actively identify and surface open questions. Key principles:

- **Distinguish placeholders from questions** — "TBD" is not necessarily an open question
- **Prioritise blocking questions** — questions that block progress over nice-to-resolve ones
- **Don't re-surface logged questions** — check if a question is already recorded before raising it
- **Offer structured parameters** — log questions with Options and Impact fields when possible
- **Respect deferrals** — if a user wants to skip a question, don't insist on logging it
- **Present at re-entry** — open questions are natural conversation starters at session start

---

## Contributing

This guide and the core specification are open for feedback. For now, proposals and discussion are welcome as GitHub issues. Formal governance processes will be established as the community grows.
