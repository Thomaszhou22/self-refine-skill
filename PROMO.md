# Self-Refine Skill - Promotional Content

## Hacker News

**Title:**
```
Show HN: Implemented self-reflection paper so AI agents can critique and improve their own outputs
```

**URL:** `https://github.com/Thomaszhou22/self-refine-skill`

**Text:**
```
Implemented the "Self-Refine: Iterative Refinement with Self-Feedback" paper (Madaan et al., 2023) as a skill for AI coding agents.

The workflow: GENERATE → CRITIQUE → REFINE → CHECK

The agent writes something, then critiques it against specific criteria, then refines based on the critique, then checks if it's good enough. Can loop up to 3 times.

Works with Claude Code, Cursor, Copilot, Codex CLI, and 10+ other AI tools. Zero API cost - just uses the same model you're already running.

Surprisingly effective for complex tasks where the first attempt is usually incomplete. The self-critique step catches edge cases, missing error handling, and incomplete implementations that I would normally have to prompt for manually.

Open source, MIT licensed. Feedback welcome.
```

## Reddit

### r/LocalLLaMA
**Title:** Implemented a self-reflection loop for AI coding agents - they critique and improve their own work

**Body:**
```
Been using this skill that makes AI agents self-reflect on their outputs. The workflow:

1. Agent generates code/solution
2. Agent critiques it (what's missing? what's wrong?)
3. Agent refines based on critique
4. Agent checks if it's good enough

Can loop up to 3 times. Based on the "Self-Refine" paper from 2023.

Works with Claude Code, Cursor, Copilot, Codex CLI, OpenClaw, and pretty much any AI tool that supports markdown instructions.

The interesting part: it catches a lot of issues I'd normally have to manually prompt for - edge cases, error handling, incomplete implementations. The self-critique step is surprisingly thorough.

Zero API cost since it uses the same model you're already running.

Open source: https://github.com/Thomaszhou22/self-refine-skill

Anyone else using self-reflection patterns with their AI agents?
```

### r/ChatGPT
**Title:** Built a skill that makes AI coding agents critique and improve their own work automatically

**Body:**
```
Implemented a research paper (Self-Refine, 2023) as a skill for AI coding assistants.

The agent writes something, then critiques it, then refines it, then checks if it's good enough. Loops up to 3 times.

Works with Claude Code, Cursor, Copilot, Codex CLI, and other AI tools. Zero extra API cost.

Example: Ask it to write a function. First attempt is usually incomplete. After self-critique and refinement, it catches edge cases, adds error handling, and improves the implementation.

Open source: https://github.com/Thomaszhou22/self-refine-skill

Pretty useful for complex tasks where you don't want to manually prompt for improvements.
```

### r/ClaudeAI
**Title:** Self-reflection skill for Claude Code - implements the Self-Refine paper

**Body:**
```
Built a skill for Claude Code that implements the "Self-Refine: Iterative Refinement with Self-Feedback" workflow:

GENERATE → CRITIQUE → REFINE → CHECK

Claude writes something, critiques it against specific criteria, refines based on the critique, then checks if it meets quality standards. Can loop up to 3 times.

Works with Claude Code, Cursor, Copilot, Codex CLI, OpenClaw, and other AI tools that support markdown instructions.

The self-critique step is surprisingly effective - catches edge cases, missing error handling, incomplete implementations that I'd normally have to manually prompt for.

Open source: https://github.com/Thomaszhou22/self-refine-skill

Thought this might be useful for the Claude Code community.
```

## Twitter/X

**Post:**
```
Implemented the Self-Refine paper for AI coding agents.

Workflow: GENERATE → CRITIQUE → REFINE → CHECK

Agent critiques its own work, catches edge cases, improves implementation. Loops up to 3 times.

Works with Claude Code, Cursor, Copilot, Codex CLI, 10+ platforms.

Open source: https://github.com/Thomaszhou22/self-refine-skill

#AI #Coding #LLM
```

## Product Hunt

**Tagline:**
Self-reflection for AI coding agents - they critique and improve their own work

**Description:**
```
Self-Refine implements the "Self-Refine: Iterative Refinement with Self-Feedback" research paper as a skill for AI coding agents.

The workflow:
1. Agent generates code/solution
2. Agent critiques it (what's missing? what's wrong?)
3. Agent refines based on critique
4. Agent checks if it's good enough
5. Loops up to 3 times

Works with Claude Code, Cursor, Copilot, Codex CLI, OpenClaw, and 10+ other AI tools. Zero API cost - uses the same model you're already running.

The self-critique step catches edge cases, error handling issues, and incomplete implementations that you'd normally have to manually prompt for.

Open source, MIT licensed.
```
