# Changelog

All notable changes to the Self-Refine Reflection Skill.

## [2.0.0] - 2026-05-19

### Added
- Pure Self-Refine system — removed all generic anti-fluff rules
- Core loop: GENERATE → CRITIQUE → REFINE → CHECK
- 6 reflection dimensions with severity ratings
- 4 depth levels (Level 0-3, auto-selected by task complexity)
- Inline reflection templates for platforms without file access
- Platform auto-detection at skill load time
- Reflexion Memory for cross-session learning (Shinn et al. 2023)
- Convergence rules (max 3 rounds, diminishing returns)
- Cost control strategy (0-50% token overhead by depth)
- Ready-to-use reflection prompt templates per depth level
- Verification question template for factual claims
- 14 platform compatibility with installation instructions
- Benchmark report (20 questions, 5 categories, +21.2% improvement)
- Before/After examples in README (code, architecture, explanation)

### Changed
- Renamed from "No-Fluff Skill" to "Self-Refine Reflection Skill"
- Repository renamed to `self-refine-skill`
- README redesigned with collapsible sections and badges
- All category JSON filenames converted to English

### References Added
- Madaan et al. (2023) — Self-Refine
- Shinn et al. (2023) — Reflexion
- Dhuliawala et al. (2023) — Chain-of-Verification
- Kadavath et al. (2022) — Self-Calibration
- Gou et al. (2023) — CRITIC
- Ng (2024) — Agentic Design Patterns

## [1.0.0] - 2026-05-19

### Added
- Initial release as "No-Fluff Skill"
- Generic anti-verbosity rules
- Basic self-check checklist
- 23 source references
