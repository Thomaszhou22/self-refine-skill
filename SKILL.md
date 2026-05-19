# Self-Refine Reflection Skill

> An AI agent skill for systematic self-reflection and iterative output refinement.
> Based on Madaan et al. (2023), Shinn et al. (2023), and Andrew Ng's Reflection design pattern.

## Platform Auto-Detection

At skill load time, detect your runtime environment and adjust capabilities:

| Capability | How to Check | Fallback
|-----------|-------------|----------|
| File system | Can you read `references/reflection-templates.md`? | Use the inline templates below instead
| Persistent memory | Can you write to `memory/`? | Store reflection notes in conversation context only
| Long context | Is your context window > 32K tokens? | Cap at Level 2 (skip adversarial review)
| Tool access | Can you call external tools? | Use mental verification only

**Detection rules:**
- If you can read this file's `references/` directory → full mode (all levels + memory)
- If you can read files but not write → full levels, in-conversation memory only
- If you cannot read files at all → use inline templates (copied below), cap at Level 2
- If context is limited (< 8K usable) → default to Level 1, max Level 2

This means **every platform gets the best possible experience automatically** — no manual configuration needed.

## Inline Reflection Templates (for environments without file access)

If you cannot read `references/reflection-templates.md`, use these directly:

### Level 1 Internal Prompt
```
Review drafted response: (1) Did I answer everything? (2) Logic gaps? (3) Can I cut 20%? (4) User can act on this? Fix → Deliver.
```

### Level 2 Internal Prompt
```
For each dimension (Logic / Facts / Completeness / Conciseness / Actionability / Consistency):
  - Quote exact sentences with issues
  - Rate: Critical / Minor / Pass
Fix ALL Critical. Fix Minor if straightforward. Re-read once. Deliver.
```

### Level 3 Internal Prompt
```
Round 1: Level 2 review.
Round 2: "If a domain expert attacked this, what would they target?" Fix valid attacks.
Round 3: Did fixes introduce new issues? If stable → deliver. If not → fix and deliver.
```

## When to Activate

Activate when you are about to deliver a final response to the user, **after** you've gathered all information and formed your answer. Reflection happens *before* delivery, not instead of work.

## Core Loop: GENERATE → CRITIQUE → REFINE → CHECK

```
1. GENERATE  — Produce your initial response as normal
2. CRITIQUE  — Apply the reflection framework (depth-dependent)
3. REFINE    — Fix every issue found in CRITIQUE
4. CHECK     — If issues remain AND within budget, loop to step 2
              — If clear OR budget exhausted, deliver
```

Source: Madaan et al., "Self-Refine: Iterative Refinement with Self-Feedback" (2023) — the same LLM acts as generator, critic, and refiner.

---

## Reflection Dimensions

Every critique examines the response across these dimensions. Not all apply to every response — skip irrelevant ones silently.

| # | Dimension | What to Check | Signal Words |
|---|-----------|---------------|--------------|
| 1 | **Logical Completeness** | Does the reasoning chain have gaps? Are conclusions supported by premises? | "therefore" without prior evidence, jumps in logic |
| 2 | **Factual Accuracy** | Are there unverified assertions? Claims that could be wrong? | Specific numbers, dates, names, "everyone knows..." |
| 3 | **Response Completeness** | Did I address every part of the user's question? Are there sub-questions I ignored? | Multiple questions in user message, implicit needs |
| 4 | **Conciseness** | Is there redundancy? Can paragraphs be merged? Are there filler phrases? | "It's important to note that", "In conclusion", repeated points |
| 5 | **Actionability** | Can the user act on this immediately? Or do they need to ask follow-ups? | Vague advice without steps, missing specifics |
| 6 | **Internal Consistency** | Do any parts of my response contradict each other? | Conflicting recommendations, contradictory statements |

---

## Reflection Depth Levels

The depth is determined by **task complexity**, not user preference. The agent auto-selects.

### Level 0: Quick Scan (Skip formal reflection)

**Trigger:** Simple factual lookups, greetings, trivial questions, single-sentence answers.

**Action:** Do nothing extra. Just respond. Cost: 0 additional tokens.

**Examples:** "What time is it?", "Thanks", simple formatting requests.

### Level 1: Standard Review

**Trigger:** Medium-complexity tasks — explanations, how-to guides, code snippets, multi-paragraph responses.

**Action:** One pass through all 6 dimensions mentally. Fix issues. Deliver.

**Budget:** 1 refinement round. ~15-20% overhead on response tokens.

**Internal process:**
```
After drafting your response, ask yourself:
- Did I answer everything they asked?
- Any logical gaps or contradictions?
- Can I cut 20% of the words without losing meaning?
- Would the user know exactly what to do next?
Fix → Deliver.
```

### Level 2: Deep Audit

**Trigger:** Complex tasks — technical architectures, multi-step plans, research summaries, anything with 5+ distinct claims.

**Action:** Explicit dimension-by-dimension review. One full refinement round with documented issues.

**Budget:** Up to 2 refinement rounds. ~30% overhead.

**Internal process:**
```
Draft response.
For each dimension (1-6):
  - Identify specific issues (quote the exact sentence)
  - Rate severity: Critical / Minor / Pass
If any Critical: refine entire response, then re-check
If only Minor: fix inline, deliver
```

### Level 3: Adversarial Review

**Trigger:** High-stakes tasks — production code, security decisions, medical/legal adjacent, public-facing content, or user explicitly requests thorough review.

**Action:** Full audit PLUS an adversarial pass where you actively try to find the weakest point in your response.

**Budget:** Up to 3 refinement rounds. ~50% overhead.

**Internal process:**
```
Draft response.
Round 1: Standard dimension-by-dimension review.
Round 2: Adversarial pass — "If I wanted to prove this response wrong, 
         what would I attack?" Check those attack vectors.
Round 3 (if needed): Verify fixes from Round 2 didn't introduce new issues.
```

---

## Convergence Rules

Based on the empirical finding from Madaan et al. that "most gains are in the initial iterations":

1. **Maximum 3 refinement rounds** regardless of depth level.
2. **Stop early if** no Critical or Minor issues found in a round.
3. **Stop early if** the changes between rounds are purely stylistic (no substantive improvement).
4. **Diminishing returns rule**: If Round N fixes fewer issues than Round N-1, stop after N.

**Anti-pattern — DO NOT:**
- Refine forever trying to reach perfection
- Make changes just to justify another round
- Re-introduce previously fixed issues

---

## Cost Control Strategy

| Depth | Max Rounds | Approx. Token Overhead | When to Use |
|-------|-----------|----------------------|-------------|
| Level 0 | 0 | 0% | Simple Q&A |
| Level 1 | 1 | ~15% | Most conversations |
| Level 2 | 2 | ~30% | Complex technical |
| Level 3 | 3 | ~50% | High-stakes only |

**Principle:** Reflection should cost less than the cost of delivering a wrong answer. For low-stakes responses, skip reflection entirely.

---

## Trigger Conditions Summary

### Auto-Trigger (always on)
- Response exceeds 3 paragraphs → at least Level 1
- Response makes 3+ factual claims → at least Level 1
- Response includes code → at least Level 1 (check logic + actionability)

### Skip Reflection
- User is in a hurry (explicit: "quick", "brief", "just tell me")
- Response is under 2 sentences
- Pure social/chat exchange

### User Manual Trigger
- User says "think carefully", "double-check", "make sure this is right" → Level 2+
- User says "this is important", "critical", "production" → Level 3
- User says "reflect" or "self-refine" → Level 2+

---

## Output Format

Reflection is **internal** — the user should not see the raw critique. However:

### After refinement, you MAY append a subtle note:

**Level 1:** No note (keep it invisible).
**Level 2:** Optionally: `_(response refined via self-review)_`
**Level 3:** Optionally: `_(refined through [N]-round adversarial self-review)_`

### NEVER:
- Show the actual critique/feedback text to the user
- Make the note prominent or distracting
- Add notes for Level 0 or Level 1 responses

---

## Reflexion Memory (Cross-Session Learning)

Inspired by Shinn et al. (2023) — Reflexion stores verbal self-reflections in persistent memory for future tasks.

**When to use:** After completing a Level 2 or Level 3 reflection where you discovered a significant pattern in your own errors (e.g., "I tend to forget edge cases in X", "I consistently over-explain Y").

**How to use:** Write a brief note to `memory/` capturing the pattern:
```
Self-reflection: When writing about [topic], I tend to [pattern]. 
Fix: [specific behavior change]. 
Date: [today].
```

This creates a growing corpus of self-corrections that improve future responses.

---

## Relationship to Other Reflection Techniques

This skill synthesizes multiple academic approaches:

| Technique | Source | What We Borrow |
|-----------|--------|---------------|
| **Self-Refine** | Madaan et al. 2023 | Core GENERATE→CRITIQUE→REFINE loop |
| **Reflexion** | Shinn et al. 2023 | Persistent memory of self-reflections |
| **Chain-of-Verification** | Dhuliawala et al. 2023 | Verification questions for factual claims (Level 2-3) |
| **Self-Calibration** | Kadavath et al. 2022 | Confidence assessment of own outputs |
| **Andrew Ng's Reflection** | Ng 2024 | Design pattern: agent critiques and improves its own output iteratively |
| **CRITIC** | Gou et al. 2023 | Tool-interactive critiquing (use tools to verify when possible) |

---

## Quick Reference Card

```
REFLECT? ──→ Simple/trivial? ──→ NO → Just respond
    │
    YES
    │
    ├─ Medium complexity → LEVEL 1 (1 round, mental check)
    ├─ Complex / multi-claim → LEVEL 2 (2 rounds, explicit review)
    └─ High-stakes / user requested → LEVEL 3 (3 rounds, adversarial)
    
For each round:
  1. Check 6 dimensions
  2. Fix all issues
  3. Converged? → Deliver
  4. Budget left? → Next round
  5. Budget exhausted → Deliver current version
```
