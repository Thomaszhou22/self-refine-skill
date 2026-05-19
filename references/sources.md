# Self-Refine Reflection — Academic Sources

## Core Papers

### Self-Refine: Iterative Refinement with Self-Feedback
- **Authors:** Madaan, Tandon, Gupta, Hallinan, Gao, Wiegreffe, Alon, Dziri, Prabhumoye, Yang, Gupta, Majumder, Hermann, Welleck, Yazdanbakhsh, Clark
- **Year:** 2023
- **URL:** https://arxiv.org/abs/2303.17651
- **Code:** https://github.com/madaan/self-refine
- **Key finding:** ~20% absolute improvement across 7 diverse tasks. Same LLM acts as generator, critic, and refiner. Most gains in initial iterations.
- **What we use:** Core GENERATE→CRITIQUE→REFINE loop, convergence rules, cost budgeting.

### Reflexion: Language Agents with Verbal Reinforcement Learning
- **Authors:** Shinn, Labash, Narasimhan
- **Year:** 2023 (NeurIPS)
- **URL:** https://arxiv.org/abs/2303.11366
- **Code:** https://github.com/noahshinn/reflexion
- **Key finding:** Agents that store verbal self-reflections in persistent memory improve across episodes without parameter updates.
- **What we use:** Cross-session reflection memory pattern.

### Chain-of-Verification Reduces Hallucination in Large Language Models
- **Authors:** Dhuliawala, Kordi, Khashabi, Schubotz, Azer, Gurevych
- **Year:** 2023
- **URL:** https://arxiv.org/abs/2309.11495
- **What we use:** Verification question pattern for factual claims in Level 2-3 reviews.

### Language Models (Mostly) Know What They Know
- **Authors:** Kadavath et al.
- **Year:** 2022
- **URL:** https://arxiv.org/abs/2207.05221
- **What we use:** Self-calibration — models can assess their own confidence.

### CRITIC: Large Language Models Can Self-Correct with Tool-Interactive Critiquing
- **Authors:** Gou et al.
- **Year:** 2023
- **What we use:** Tool-assisted verification (use search, code execution, etc. to verify claims).

## Design Patterns

### Andrew Ng — Agentic Design Patterns: Reflection
- **Year:** 2024
- **Source:** LinkedIn / X posts, AI India keynote
- **Key insight:** Reflection is the first and most impactful agentic design pattern. Agent critiques its own output and improves iteratively.
- **Recommended papers by Ng:** Self-Refine (Madaan), Reflexion (Shinn), CRITIC (Gou)

## Related Techniques

### Self-Consistency: Improves Chain of Thought Reasoning
- **Authors:** Wang et al.
- **Year:** 2022
- **URL:** https://arxiv.org/abs/2203.11171

### Tree of Thoughts: Deliberate Problem Solving
- **Authors:** Yao et al.
- **Year:** 2023
- **URL:** https://arxiv.org/abs/2305.10601

### Cumulative Reasoning with Large Language Models
- **Authors:** Zhang et al.
- **Year:** 2023
- **URL:** https://arxiv.org/abs/2308.04371

## Practical References

### Learn Prompting — Self-Criticism Section
- **URL:** https://learnprompting.org/docs/advanced/self_criticism/introduction
- **Covers:** Self-Calibration, Self-Refine, RCoT, Self-Verification, CoVe, Cumulative Reasoning

### Self-Refine Demo Site
- **URL:** https://selfrefine.info/
