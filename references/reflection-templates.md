# Reflection Prompt Templates

Internal prompt templates the agent uses during self-review. These are NOT shown to users.

---

## Level 1: Standard Review Prompt (Internal)

```
Review your drafted response across these dimensions. For each, mark Pass/Fix:

1. COMPLETENESS: Did I address every part of the user's request?
2. LOGIC: Any unsupported conclusions or logical gaps?
3. ACCURACY: Any claims I'm not confident about?
4. CONCISENESS: Can I cut 20% without losing meaning?
5. ACTIONABILITY: Can the user act on this immediately?
6. CONSISTENCY: Any internal contradictions?

For any "Fix" items, revise the response now. Then deliver.
```

---

## Level 2: Deep Audit Prompt (Internal)

```
You are conducting a structured self-review of your drafted response.

## Step 1: Dimension Review
For each dimension, identify SPECIFIC issues (quote exact sentences):

### Logical Completeness
- [Quote any unsupported leap or gap in reasoning]
- Severity: Critical / Minor / Pass

### Factual Accuracy
- [Quote any unverified or potentially wrong claims]
- Severity: Critical / Minor / Pass

### Response Completeness
- [List any parts of the user's question not addressed]
- Severity: Critical / Minor / Pass

### Conciseness
- [Quote any redundant sections or filler]
- Severity: Critical / Minor / Pass

### Actionability
- [Quote any vague advice that lacks specific next steps]
- Severity: Critical / Minor / Pass

### Internal Consistency
- [Quote any contradictory statements]
- Severity: Critical / Minor / Pass

## Step 2: Refine
Fix ALL Critical issues. Fix Minor issues if straightforward.

## Step 3: Verify
Re-read the refined response. Did the fixes introduce new issues?
If yes → fix those too.
If no → deliver.
```

---

## Level 3: Adversarial Review Prompt (Internal)

```
You are conducting an adversarial self-review. This is a high-stakes response.

## Round 1: Structured Review
[Apply the Level 2 Deep Audit template above]

## Round 2: Adversarial Attack
Now switch to adversarial mode. Your goal: find the WEAKEST point in your response.

Ask yourself:
- "If a domain expert read this, what would they criticize?"
- "What assumption am I making that could be wrong?"
- "What edge case does this not handle?"
- "What would a skeptical reader challenge?"
- "Is there any claim here that I cannot back up?"

For each attack vector found:
1. State the attack: "A critic would say..."
2. Evaluate: Is this a valid criticism? (Yes/No/Partially)
3. If valid: Revise to address it.

## Round 3: Stability Check
- Did Round 2 fixes introduce any new issues?
- Re-scan for consistency and completeness.
- If stable → deliver.
- If new Critical issues → fix and deliver (do NOT start another round).
```

---

## Verification Question Template (for factual claims)

Used in Level 2-3 when the response contains specific factual claims.

```
For each factual claim in my response:
1. State the claim: [exact quote]
2. Ask: "How do I know this is true?"
3. Can I verify it? (existing knowledge / tool use needed / uncertain)
4. If uncertain → either remove the claim, hedge it, or flag it.
```

---

## Reflexion Memory Template

Used when a significant self-correction pattern is discovered.

```markdown
## Self-Reflection Note
- **Date:** [YYYY-MM-DD]
- **Context:** [What task triggered this reflection]
- **Pattern discovered:** [What error pattern I found in myself]
- **Root cause:** [Why I think this happens]
- **Behavior change:** [What I'll do differently]
```

Write to `memory/reflections.md` (append, don't overwrite).
