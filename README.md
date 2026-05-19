<div align="center">

# 🪞 Self-Refine Skill

**Systematic self-reflection for AI agents.**

*Make every AI response better — before the user sees it.*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-compatible-blue)](https://openclaw.ai)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-purple)](https://claude.ai)
[![Cursor](https://img.shields.io/badge/Cursor-compatible-orange)](https://cursor.sh)
[![GitHub Copilot](https://img.shields.io/badge/Copilot-compatible-lightblue)](https://github.com/features/copilot)
[![Codex CLI](https://img.shields.io/badge/Codex%20CLI-compatible-green)](https://github.com/openai/codex)
[![Gemini CLI](https://img.shields.io/badge/Gemini%20CLI-compatible-4285F4)](https://ai.google.dev)
[![Windsurf](https://img.shields.io/badge/Windsurf-compatible-00D4AA)](https://codeium.com/windsurf)
[![JetBrains](https://img.shields.io/badge/JetBrains-compatible-000)](https://www.jetbrains.com/ai/)
[![Benchmarked](https://img.shields.io/badge/benchmarked-21%25%20improvement-green)](BENCHMARK.md)

</div>

---

## What It Does

Most AI responses are first drafts. This skill adds an **invisible self-review step** before every response:

```
GENERATE → CRITIQUE → REFINE → CHECK → DELIVER
```

Based on peer-reviewed research showing **~20% quality improvement** from iterative self-refinement (Madaan et al., 2023).

**One file. Zero dependencies. Zero API cost.** The host AI acts as its own critic.

---

## Proven Results

Benchmarked on **20 questions across 5 categories** with controlled before/after evaluation. → [Full report](BENCHMARK.md)

| Metric | Result |
|--------|--------|
| **Overall improvement** | **+21.2%** |
| Questions improved | 20/20 (100%) |
| Best single improvement | +43.9% |

**Per-dimension improvement:**

| Dimension | Δ |
|-----------|---|
| Conciseness | +27.7% |
| Actionability | +25.0% |
| Response Completeness | +24.8% |
| Logical Completeness | +23.7% |
| Factual Accuracy | +14.8% |
| Internal Consistency | +11.9% |

📖 [Read the full benchmark report →](BENCHMARK.md)

---

## Before & After

<details>
<summary><b>Example 1 — Code Debugging</b></summary>

**User:** *"My React app crashes on page load. Error: `Cannot read properties of undefined (reading 'map')`"*

**❌ Without Self-Refine:**
> This error typically occurs when you try to call `.map()` on an undefined value. In React, this usually happens when you're trying to render a list but the data hasn't loaded yet. You should check if the variable is defined before mapping over it. You can use optional chaining or a conditional check. Here's an example:
> ```jsx
> {data && data.map(item => <div key={item.id}>{item.name}</div>)}
> ```
> Also, make sure your state is initialized properly...

**✅ With Self-Refine:**
> The crash is `.map()` on `undefined`. Two fixes:
>
> **Quick fix:**
> ```jsx
> {data?.map(item => <div key={item.id}>{item.name}</div>)}
> ```
> **Root cause:**
> ```jsx
> const [data, setData] = useState([]);  // not useState() or null
> ```
> Likely `useState()` returns `undefined` → `.map()` throws.

</details>

<details>
<summary><b>Example 2 — Architecture Decision</b></summary>

**User:** *"Redis or PostgreSQL for session storage?"*

**❌ Without Self-Refine:**
> Both are great options and the choice depends on your needs. Redis is fast and in-memory. PostgreSQL is durable but slower. Redis is generally preferred for sessions. But if you already have PostgreSQL, it works fine too. Consider speed, persistence, and complexity.

**✅ With Self-Refine:**
> **Redis.** Here's why:
>
> | | Redis | PostgreSQL |
> |---|---|---|
> | Latency | ~0.1ms | ~2-5ms |
> | TTL | Built-in | Manual cleanup |
> | Scale | Millions, no schema cost | Row bloat, VACUUM |
>
> Use PostgreSQL only if <100 users and you don't want Redis.
>
> ```bash
> npm install connect-redis express-session
> ```

</details>

<details>
<summary><b>Example 3 — Concise Explanation</b></summary>

**User:** *"Explain JWT in 3 sentences."*

**❌ Without Self-Refine:**
> JWT (JSON Web Token) is a popular method for handling authentication in web applications. It works by creating a signed token that contains user information and is sent with each request. The server can verify the token without needing to store session data, making it stateless and scalable. JWTs consist of three parts: a header, a payload, and a signature, all encoded in Base64...

**✅ With Self-Refine:**
> JWT is a signed JSON token (`header.payload.signature`) proving identity. After login, the server issues one; the client sends it in `Authorization: Bearer <token>` on every request. The server verifies the signature — no session storage needed.

</details>

---

## How It Works

### 6 Reflection Dimensions

| # | Dimension | Catches |
|---|-----------|---------|
| 1 | **Logical Completeness** | Broken reasoning chains |
| 2 | **Factual Accuracy** | Unverified claims |
| 3 | **Response Completeness** | Ignored sub-questions |
| 4 | **Conciseness** | Redundancy, filler |
| 5 | **Actionability** | Vague advice without steps |
| 6 | **Internal Consistency** | Self-contradictions |

### 4 Depth Levels (Auto-Selected)

| Level | When | Rounds | Token Cost |
|:-----:|------|:------:|:----------:|
| **0** | Simple Q&A | 0 | +0% |
| **1** | Most conversations | 1 | ~15% |
| **2** | Complex technical | 2 | ~30% |
| **3** | High-stakes / adversarial | 3 | ~50% |

No configuration. The agent picks the right depth automatically based on task complexity.

### Cross-Session Learning (Reflexion)

When the agent discovers a recurring error pattern during self-review, it saves a note to memory. Next session, it avoids the same mistake. Based on Shinn et al. (2023).

---

## Installation

<details>
<summary><b>OpenClaw (recommended)</b></summary>

```bash
# Copy to your skills directory
cp -r self-refine-skill/ ~/.openclaw/skills/
```

That's it. OpenClaw auto-detects skills.

</details>

<details>
<summary><b>Claude Code</b></summary>

```bash
# Copy to your project
cp -r self-refine-skill/ skills/

# Add to CLAUDE.md
echo "Read and follow skills/self-refine-skill/SKILL.md" >> CLAUDE.md
```

</details>

<details>
<summary><b>Cursor</b></summary>

```bash
# Copy to project root
cp -r self-refine-skill/ skills/

# Add to .cursorrules
echo "Read and follow skills/self-refine-skill/SKILL.md" >> .cursorrules
```

</details>

<details>
<summary><b>Gemini CLI</b></summary>

```bash
# Copy to project, add to GEMINI.md
cp -r self-refine-skill/ skills/
echo "Read and follow skills/self-refine-skill/SKILL.md" >> GEMINI.md
```

</details>

<details>
<summary><b>GitHub Copilot</b></summary>

```bash
# Add to your repository's Copilot instructions
mkdir -p skills && cp -r self-refine-skill/ skills/
echo "Read and follow skills/self-refine-skill/SKILL.md" >> .github/copilot-instructions.md
```

</details>

<details>
<summary><b>Codex CLI (OpenAI)</b></summary>

```bash
cp -r self-refine-skill/ skills/
echo "Read and follow skills/self-refine-skill/SKILL.md" >> AGENTS.md
```

</details>

<details>
<summary><b>Windsurf</b></summary>

```bash
cp -r self-refine-skill/ skills/
echo "Read and follow skills/self-refine-skill/SKILL.md" >> .windsurfrules
```

</details>

<details>
<summary><b>Cline / AI Coding Assistants</b></summary>

Copy `self-refine-skill/` to your project. Reference `SKILL.md` in your assistant's custom instructions.

</details>

<details>
<summary><b>JetBrains AI / Junie</b></summary>

1. Open Settings → Tools → AI Assistant → System Instructions
2. Add: `Read and follow skills/self-refine-skill/SKILL.md`
3. Place the skill folder in your project root

</details>

<details>
<summary><b>Zed</b></summary>

1. Open Zed settings
2. Add to your context or assistant instructions:
```
Read and follow skills/self-refine-skill/SKILL.md
```

</details>

<details>
<summary><b>Kiro</b></summary>

1. Add skill folder to project
2. Reference in Kiro's instruction configuration

</details>

<details>
<summary><b>OpenCode</b></summary>

1. Add skill folder to project
2. Add to `AGENTS.md`: `Read and follow skills/self-refine-skill/SKILL.md`

</details>

<details>
<summary><b>ChatGPT Custom GPT</b></summary>

1. Open your GPT → Settings → Instructions
2. Paste the contents of `SKILL.md`
3. Save

**Note:** SKILL.md includes inline templates for environments without file access — all depth levels work. Reflection memory is in-conversation only (no persistence).

</details>

<details>
<summary><b>Any AI Tool</b></summary>

Copy `SKILL.md` into your system prompt or instructions file. That's the only file you need.

</details>

---

## Platform Compatibility

**Top platforms (most popular by usage):**

| Platform | Rating | Reflexion Memory | Notes |
|----------|:------:|:----------------:|-------|
| **Claude Code** | ⭐⭐⭐⭐⭐ | ✅ File write | #1 coding agent 2026, full support |
| **Cursor** | ⭐⭐⭐⭐⭐ | ✅ File write | $2B ARR AI IDE, full support |
| **GitHub Copilot** | ⭐⭐⭐⭐⭐ | ✅ File write | Largest user base, full support |
| **Codex CLI** | ⭐⭐⭐⭐⭐ | ✅ File write | OpenAI's coding agent, full support |
| **ChatGPT** | ⭐⭐⭐⭐ | ⚠️ In-conversation | Inline templates auto-load, all levels |

<details>
<summary><b>Show all 14 supported platforms</b></summary>

| Platform | Rating | Reflexion Memory | Install |
|----------|:------:|:----------------:|---------|
| **OpenClaw** | ⭐⭐⭐⭐⭐ | ✅ `memory/` dir | Copy to `skills/`, auto-detect |
| **Claude Code** | ⭐⭐⭐⭐⭐ | ✅ File write | Copy to project, add to `CLAUDE.md` |
| **Cursor** | ⭐⭐⭐⭐⭐ | ✅ File write | Copy to project, add to `.cursorrules` |
| **GitHub Copilot** | ⭐⭐⭐⭐⭐ | ✅ File write | Add to `.github/copilot-instructions.md` |
| **Codex CLI** | ⭐⭐⭐⭐⭐ | ✅ File write | Add to `AGENTS.md` |
| **Gemini CLI** | ⭐⭐⭐⭐⭐ | ✅ File write | Add to `GEMINI.md` |
| **Windsurf** | ⭐⭐⭐⭐⭐ | ✅ File write | Add to `.windsurfrules` |
| **Cline** | ⭐⭐⭐⭐⭐ | ✅ File write | Add to custom instructions |
| **JetBrains AI / Junie** | ⭐⭐⭐⭐½ | ✅ File write | Add to AI Assistant instructions |
| **Aider** | ⭐⭐⭐⭐ | ✅ File write | Place in project, reference with `--file` |
| **Zed** | ⭐⭐⭐⭐ | ✅ File write | Add to Zed settings |
| **ChatGPT GPT** | ⭐⭐⭐⭐ | ⚠️ In-conversation | Paste SKILL.md into GPT Instructions |
| **Kiro** | ⭐⭐⭐⭐ | ✅ File write | Add to Kiro instructions |
| **OpenCode** | ⭐⭐⭐⭐ | ✅ File write | Add to AGENTS.md |

</details>

---

## Academic Foundations

| Paper | Year | What We Use |
|-------|:----:|-------------|
| **Self-Refine** — Madaan et al. | 2023 | Core GENERATE→CRITIQUE→REFINE loop |
| **Reflexion** — Shinn et al. | 2023 | Persistent self-reflection memory |
| **Chain-of-Verification** — Dhuliawala et al. | 2023 | Verification questions for facts |
| **Self-Calibration** — Kadavath et al. | 2022 | Confidence assessment |
| **CRITIC** — Gou et al. | 2023 | Tool-interactive self-correction |
| **Agentic Design Patterns** — Andrew Ng | 2024 | Reflection as foundational pattern |

Full bibliography: [`references/sources.md`](references/sources.md)

---

## File Structure

```
self-refine-skill/
├── SKILL.md                          # Core instructions (the only required file)
├── README.md                         # This file
├── LICENSE                           # MIT
└── references/
    ├── sources.md                    # Academic sources with URLs
    └── reflection-templates.md       # Ready-to-use internal prompts per depth level
```

---

<div align="center">

**Good answers are like good code: no unnecessary lines, every line serves a purpose.**

⭐ Star this repo if it improved your AI's output quality.

</div>
