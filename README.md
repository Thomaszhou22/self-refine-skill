# No-Fluff Skill

An OpenClaw skill that enforces concise, high-quality AI output by eliminating verbosity, sycophancy, and fluff.

## What It Does

- **Removes filler**: No "Great question!", no "Hope this helps!", no unnecessary transitions
- **Enforces precision**: Specific over vague, "I don't know" over hallucination
- **Structures output**: Conclusions first, lists over paragraphs, code without preamble
- **Built-in reflection**: Internal self-check before every response (based on Self-Refine research)
- **Adaptive**: Different conciseness levels for code, debugging, explanations, and chat

## Installation

1. Copy the `no-fluff/` directory into your OpenClaw `skills/` folder
2. The skill activates when:
   - User says "no fluff", "别废话", or "简洁模式"
   - Or when loaded as an always-on skill via configuration

## Design Philosophy

This skill is based on research from 20+ sources including:

- **Official guides**: OpenAI, Anthropic (Claude), Microsoft Azure prompt engineering docs
- **Academic papers**: Self-Refine (Madaan et al., 2023), Chain-of-Thought (Wei et al., 2022), Reflexion
- **Industry experts**: Andrew Ng's Agentic Design Patterns, Paul Graham's "Write Simply"
- **Technical writing**: Google Technical Writing Guide — conciseness principles
- **Hallucination prevention**: Anthropic's anti-hallucination strategies, layered defense approaches

## Key Principle

> **Good answers are like good code: no unnecessary lines, every line serves a purpose.**
