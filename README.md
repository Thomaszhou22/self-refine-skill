# Self-Refine Reflection Skill

> **The single highest-leverage technique for improving AI output quality.**

An OpenClaw AgentSkill that implements systematic self-reflection and iterative refinement for AI agents, grounded in peer-reviewed research.

## Why Self-Refine?

Andrew Ng identified **Reflection** as the first of four key agentic design patterns. The research is clear: LLMs produce significantly better output when they critique and refine their own work iteratively.

- **Madaan et al. (2023)** showed ~20% absolute improvement across 7 tasks with Self-Refine
- **Shinn et al. (2023)** demonstrated that persistent self-reflection memory (Reflexion) enables learning across episodes
- The key insight: *the same model that generates can also critique and improve* — no extra training needed

This isn't about removing fluff (any prompt can do that). It's about building a **systematic, multi-dimensional self-review process** that catches logical gaps, factual errors, inconsistencies, and missing information before the user ever sees them.

## What It Does

The skill activates **before delivering a final response** and runs a structured self-review:

1. **Generate** your initial response
2. **Critique** it across 6 dimensions (logic, facts, completeness, conciseness, actionability, consistency)
3. **Refine** based on the critique
4. **Check** — if issues remain and budget allows, loop

### Four Depth Levels

| Level | When | Rounds | Overhead |
|-------|------|--------|----------|
| **Level 0** | Simple Q&A | 0 | 0% |
| **Level 1** | Most conversations | 1 | ~15% |
| **Level 2** | Complex technical | 2 | ~30% |
| **Level 3** | High-stakes / adversarial | 3 | ~50% |

Auto-selected based on task complexity. No configuration needed.

### Cross-Session Learning

Borrowing from Reflexion (Shinn et al. 2023), patterns discovered during self-review are persisted to memory, creating a growing corpus of self-corrections.

## Installation

Place in your OpenClaw `skills/` directory. No dependencies.

## Academic Foundations

- Madaan et al., "Self-Refine: Iterative Refinement with Self-Feedback" (2023)
- Shinn et al., "Reflexion: Language Agents with Verbal Reinforcement Learning" (2023)
- Dhuliawala et al., "Chain-of-Verification Reduces Hallucination" (2023)
- Kadavath et al., "Language Models (Mostly) Know What They Know" (2022)
- Gou et al., "CRITIC: Large Language Models Can Self-Correct with Tool-Interactive Critiquing" (2023)
- Andrew Ng, "Agentic Design Patterns" (2024)

## License

MIT
