# Changelog

All notable changes to the BRIEF.md specification are documented in this file.

Format based on [Keep a Changelog](https://keepachangelog.com/).

---

## [2.0] — 2026-03

### Added

- **Extended decision record fields**: REPLACES, EXCEPTION TO, AMENDS, RESOLVED FROM, SUPERSEDED BY — for linking decisions to each other with clear semantics
- **Internationalisation**: section heading aliases in German, French, Spanish, Portuguese, and Japanese, plus English aliases ("Decisions", "Questions", etc.)
- **External Tool Sessions**: convention for recording breadcrumbs from work done in external tools (Figma, DAWs, etc.)
- **Second complete example**: software project (brief-validate CLI tool) alongside the existing music example (Echo Valley)
- **Software examples** added throughout the spec where only music examples existed (Quick Start, Extensions, Project-Specific sections)
- **Ontology Pack Architecture** (planned): pack format, tagging, memory management — documented in Implementation Guide as a reference implementation feature
- **Type Guide System** (planned): domain-specific bootstrapping templates — documented in Implementation Guide as a reference implementation feature
- **Project Maturity Assessment**: lifecycle phases (nascent/developing/maturing/established) with signals for tools to calibrate guidance
- **AI Integration Patterns**: 10 interaction patterns, decision recognition principles, question surfacing principles, and session re-entry sequence
- **Conflict Detection**: concrete two-layer approach (heuristic + optional semantic) with conflict suppression via intentional tensions
- **Configuration section**: `~/.brief/` directory layout and configuration schema with defaults
- **CHANGELOG.md**: this file

### Changed

- **Version**: v1.0-draft → v2.0
- **Date**: February 2026 → March 2026
- **Hierarchy description**: clarified that hierarchy walking is opt-in at the tool level, not implied default behaviour for all tools
- **Governance language**: replaced "no formal governance yet" / "clarity over consensus" with "governance processes will be established as the community grows"
- **Version compatibility**: updated from v1.x to v2.x references; documented that v1.0 files are valid v2.0 files with no migration needed

---

## [1.0] — 2026-02

Initial draft specification.

- Core structure: metadata, five core sections, extensions, project-specific
- Folder-based hierarchy with advisory parent context
- Design principles, non-goals, validation rules
- Implementation guide for tool builders
- Extensions architecture (community-maintained, six extensions under development)
