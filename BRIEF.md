**Project:** BRIEF.md Specification
**Type:** open_standard
**Status:** development
**Created:** 2026-01-15
**Updated:** 2026-03-16
 
# What This Is
An open specification for a lightweight, human-readable context file
that preserves project intent across time, tools, and collaborators.
Defines a universal core structure (identity, boundaries, motivation,
decisions, open questions) with optional domain-specific extensions.
 
# What This Is NOT
- Not a project management system (no tasks, timelines, or owners)
- Not a comprehensive documentation platform (no wikis, no knowledge base)
- Not an AI tool or AI memory replacement (no intelligence, no inference)
- Not a database schema or knowledge graph (optimises human recall,
  not machine queries)
- Not a methodology or workflow prescription (records outputs of
  decisions, not how to make them)
- Not a replacement for README.md or agent.md (occupies a distinct,
  complementary space)
 
# Why This Exists
Tools preserve state but not intent. When work pauses and resumes,
practitioners lose awareness of why decisions were made, what
constraints mattered, and what was deliberately excluded. This cost
is invisible, unnamed, and accepted as inevitable. BRIEF.md exists
to make project intent explicit, portable, and persistent, so that
re-entry becomes continuation rather than reconstruction.
 
# Key Decisions
 
### WHAT: Markdown prose, not JSON or YAML
**WHY:** The file must be useful even if no tool ever parses it.
Human readability is the primary design constraint. Structured data
formats optimise for machines; BRIEF.md optimises for a person
returning to a project after weeks away.
**WHEN:** 2026-01-15
**ALTERNATIVES CONSIDERED:**
- JSON: rejected, unreadable by humans without tooling
- YAML: rejected, better than JSON but still feels like
  configuration, not prose
- Custom format: rejected, unnecessary friction, no ecosystem
 
### WHAT: Context hierarchy through folder nesting, not explicit linking
**WHY:** Relationships between projects should be self-evident from
structure, not maintained through configuration. A song in an album
folder inherits album context without anyone declaring the relationship.
Zero configuration means zero maintenance burden for hierarchy.
**WHEN:** 2026-01-15
**ALTERNATIVES CONSIDERED:**
- Explicit links between files: rejected, creates maintenance burden,
  links break when files move
- Configuration file declaring relationships: rejected, adds complexity,
  requires tooling to manage
- Flat structure (no hierarchy): rejected, loses relational context
  entirely
 
### WHAT: Decision records embedded in the project file, not separate
  documents
**WHY:** ADRs store each decision as a separate file in a decisions
directory. This is architecturally clean but creates friction: you
navigate away from context to write one, and there is no unified view
of the project. Embedding decisions alongside identity, constraints,
and open questions provides unified context in a single file.
**WHEN:** 2026-01-15
**ALTERNATIVES CONSIDERED:**
- Separate ADR files per decision: rejected, proven pattern but
  friction of file creation limits adoption, especially outside
  software
- Decisions in external documentation: rejected, spatial separation
  causes drift
 
### WHAT: "What This Is NOT" as a formal core section
**WHY:** Constraint context is among the fastest to decay and most
consequential to lose. Constraints during active work are often tacit
intuitions that vanish after a break. Making them explicit prevents
the most common re-entry error: changing something that seemed fine
but contradicts an original constraint. While "Non-Goals" sections
appear in some software design documentation, framing negative space
as essential context preservation (not optional scope management) is
a deliberate design choice.
**WHEN:** 2026-01-15
 
### WHAT: "To Keep Open" subsection for intentional ambiguity
**WHY:** In creative work, unresolved tensions are often features
rather than defects. No widely adopted project documentation standard
includes a dedicated section for questions that should deliberately
remain unresolved. This section gives practitioners permission to name
what should stay open, rather than treating all open questions as
problems to be solved.
**WHEN:** 2026-01-20
 
### WHAT: Domain-agnostic core with optional community-defined
  extensions
**WHY:** The same five core sections work for a song, a software
feature, a research paper, or a film scene. Domain-specific depth
is provided through extensions rather than baked into the core,
keeping the base format simple while allowing communities to add
vocabularies relevant to their domain.
**WHEN:** 2026-01-15
**ALTERNATIVES CONSIDERED:**
- Domain-specific formats from the start: rejected, fragments the
  standard, limits cross-domain portability
- No extension mechanism: rejected, core alone is too shallow for
  specialised work
- Single monolithic format covering all domains: rejected, becomes
  bloated and intimidating
 
### WHAT: Human-first, AI-readable (not AI-first)
**WHY:** Existing project-root context files (agent.md, .cursorrules,
AGENTS.md) are written as instructions for AI agents that happen to
be human-readable. BRIEF.md inverts this: written for humans in prose
with rationale, structured so AI tools can also parse it. The primary
audience for project intent is the practitioner, not the machine.
**WHEN:** 2026-01-20
**ALTERNATIVES CONSIDERED:**
- AI-first with human readability as secondary: rejected, optimises
  for the wrong audience and risks becoming brittle prompt engineering
 
### WHAT: Parent context is advisory, not prescriptive
**WHY:** Creative and exploratory work requires deviation. If parent
BRIEF.md files enforced constraints on children, the hierarchy would
prevent the very evolution it aims to support. Advisory context
informs without constraining. Tools may warn about conflicts but do
not enforce.
**WHEN:** 2026-01-20
**ALTERNATIVES CONSIDERED:**
- Prescriptive inheritance: rejected, prevents productive deviation,
  makes the hierarchy a constraint enforcer
- No parent awareness at all: rejected, loses the value of relational
  context
 
### WHAT: Colocation with the project directory, not external storage
**WHY:** Spatial separation between documentation and work is a
primary cause of documentation drift. When context lives in the same
directory as the work, it travels with the project: archiving,
sharing, and moving the project automatically preserves context.
**WHEN:** 2026-01-15
**ALTERNATIVES CONSIDERED:**
- Centralised documentation platform: rejected, creates spatial
  separation, vendor dependency
- Database or API-based storage: rejected, requires tooling, not
  human-editable
 
# Open Questions
 
## To Resolve
- [ ] Should the specification recommend a maximum file length or
      remain silent on size?
      Context: Complex projects with many decisions will naturally
      produce longer files. A hard limit risks being unrealistic;
      no guidance risks bureaucratic bloat. Current approach uses a
      readability heuristic (five minutes) rather than a line count.
- [ ] How should conflict detection between parent and child BRIEF.md
      files work?
      Context: The spec currently says "implementation-specific."
      As adoption grows, clearer guidance may be needed on what
      constitutes a conflict and how tools should surface it.
- [ ] Is the proposed extension architecture viable at scale?
      Context: No extensions have been formally defined. No
      community governance exists. The composability of a universal
      core with domain-specific vocabularies is architecturally sound
      in principle but depends on community adoption.
- [ ] Does BRIEF.md measurably reduce re-entry time after
      interruptions?
      Context: The claim that explicit context preservation helps
      is intuitively strong but empirically untested. Controlled
      studies are needed to validate, quantify, or refute the effect.
- [ ] Does AI output quality change when a BRIEF.md is available?
      Context: Anecdotal evidence suggests AI tools perform better
      with structured context, but no controlled comparison exists.
- [ ] Does externalising tacit creative intent change the nature of
      the creative work itself?
      Context: There is a genuine risk that documenting constraints
      and decisions could reduce productive ambiguity or creative
      freedom. This interaction needs investigation.
 
## To Keep Open
- Whether BRIEF.md adoption ultimately depends on tool integration
  or can sustain on manual use alone (both paths have merit and the
  answer may differ across domains and user types)
- Whether the "context decay" framing is best understood as a design
  problem, a cognitive problem, or both (the paper argues for design,
  but this is a position, not a settled finding)
- The boundary between context and instruction: where does project
  intent end and tool configuration begin? (This tension is productive
  and may not have a clean resolution)
