# Self-Refine Reflection Skill

> **The single highest-leverage technique for improving AI output quality — now an installable skill.**

An AgentSkill that implements systematic self-reflection and iterative refinement for AI agents, grounded in peer-reviewed research.

## Why Self-Refine?

Andrew Ng identified **Reflection** as the first of four key agentic design patterns. The research is clear: LLMs produce significantly better output when they critique and refine their own work iteratively.

- **Madaan et al. (2023)** showed ~20% absolute improvement across 7 tasks with Self-Refine
- **Shinn et al. (2023)** demonstrated that persistent self-reflection memory (Reflexion) enables learning across episodes
- The key insight: *the same model that generates can also critique and improve* — no extra training needed

This isn't about removing fluff (any prompt can do that). It's about building a **systematic, multi-dimensional self-review process** that catches logical gaps, factual errors, inconsistencies, and missing information before the user ever sees them.

---

## Before & After Examples

### Example 1: Code Debugging

**User:** "My React app crashes on page load. Here's the error: `Cannot read properties of undefined (reading 'map')`"

**Without Self-Refine:**
> This error typically occurs when you try to call `.map()` on an undefined value. In React, this usually happens when you're trying to render a list but the data hasn't loaded yet. You should check if the variable is defined before mapping over it. You can use optional chaining or a conditional check. Here's an example of how to fix it:
> ```jsx
> {data && data.map(item => <div key={item.id}>{item.name}</div>)}
> ```
> Also, make sure your state is initialized properly...

**With Self-Refine (Level 2):**
> The crash is on a `.map()` call where the variable is `undefined`. Two fixes:
>
> **1. Guard the render (immediate fix):**
> ```jsx
> {data?.map(item => <div key={item.id}>{item.name}</div>)}
> ```
> **2. Initialize state properly (root cause):**
> ```jsx
> const [data, setData] = useState([]);  // not useState() or useState(null)
> ```
> The root cause is likely `useState()` without an initial value — it returns `undefined`, and `.map()` on `undefined` throws.

**What changed:** Eliminated generic explanation, identified the specific root cause, gave both the fix and the root cause fix.

---

### Example 2: Architecture Advice

**User:** "Should I use Redis or PostgreSQL for session storage in my Node.js app?"

**Without Self-Refine:**
> Both Redis and PostgreSQL are great options for session storage, and the choice depends on your specific needs. Redis is an in-memory data store that's very fast, making it ideal for session data. PostgreSQL is a relational database that's more durable but slower for frequent reads/writes. Redis is generally the preferred choice for session storage because of its speed and built-in TTL support. However, if you already have PostgreSQL and don't want to add another dependency, it can work fine too. You should consider your requirements for speed, persistence, and infrastructure complexity.

**With Self-Refine (Level 2):**
> **Redis** for sessions. Here's why:
>
> | Factor | Redis | PostgreSQL |
> |--------|-------|-----------|
> | Latency | ~0.1ms | ~2-5ms |
> | TTL expiry | Built-in (`EXPIRE`) | Manual cleanup job |
> | Session scale | Millions, no schema cost | Row bloat, VACUUM overhead |
>
> **When to use PostgreSQL instead:**
> - < 100 concurrent users and you don't want to manage Redis
> - Sessions need complex queries (unlikely)
>
> **Implementation:**
> ```bash
> npm install connect-redis express-session
> ```
> ```js
> const session = require('express-session');
> const RedisStore = require('connect-redis').default;
> const { createClient } = require('redis');
>
> app.use(session({
>   store: new RedisStore({ client: createClient() }),
>   secret: process.env.SESSION_SECRET,
>   resave: false,
>   saveUninitialized: false,
> }));
> ```

**What changed:** Replaced vague pros/cons with a decision table, specific numbers, and copy-paste implementation. User can act immediately.

---

### Example 3: Technical Explanation

**User:** "Explain JWT authentication in 3 sentences."

**Without Self-Refine:**
> JWT (JSON Web Token) is a popular method for handling authentication in web applications. It works by creating a signed token that contains user information and is sent with each request. The server can verify the token without needing to store session data, making it stateless and scalable. JWTs consist of three parts: a header, a payload, and a signature, all encoded in Base64. When a user logs in, the server generates a JWT and sends it to the client, which then includes it in the Authorization header of subsequent requests.

**With Self-Refine (Level 1):**
> JWT is a signed JSON token (`header.payload.signature`) that proves identity. After login, the server issues a JWT; the client sends it in `Authorization: Bearer <token>` on every request. The server verifies the signature — no session storage needed.

**What changed:** Hit the 3-sentence constraint, removed filler, kept all essential information.

---

## How It Works

The skill activates **before delivering a final response** and runs a structured self-review:

1. **Generate** your initial response
2. **Critique** it across 6 dimensions (logic, facts, completeness, conciseness, actionability, consistency)
3. **Refine** based on the critique
4. **Check** — if issues remain and budget allows, loop

### Four Depth Levels (Auto-Selected)

| Level | When | Rounds | Overhead |
|-------|------|--------|----------|
| **Level 0** | Simple Q&A, greetings | 0 | 0% |
| **Level 1** | Most conversations | 1 | ~15% |
| **Level 2** | Complex technical | 2 | ~30% |
| **Level 3** | High-stakes / adversarial | 3 | ~50% |

No configuration needed — the agent auto-selects depth based on task complexity.

### Cross-Session Learning

Borrowing from Reflexion (Shinn et al. 2023), patterns discovered during self-review are persisted to memory, creating a growing corpus of self-corrections that improve future responses.

---

## 🖥️ Platform Compatibility

| Platform | Rating | How to Install | Notes |
|----------|--------|---------------|-------|
| **OpenClaw** | ⭐⭐⭐⭐⭐ | Place in `skills/self-refine-skill/` | Full support. Auto-detects. Reflexion memory works with `memory/` directory. |
| **Claude Code** | ⭐⭐⭐⭐ | Copy to project. Add to `CLAUDE.md`: `Read and follow skills/self-refine-skill/SKILL.md` | Full support. Reflexion memory via in-conversation notes. |
| **Cursor** | ⭐⭐⭐⭐ | Copy to project root. Add to `.cursorrules`: `Read and follow skills/self-refine-skill/SKILL.md` | Full support. No cross-session persistence. |
| **Gemini CLI** | ⭐⭐⭐½ | Copy to project, reference in system instructions | SKILL.md fits within context. Reflexion memory limited. |
| **Cline** | ⭐⭐⭐⭐ | Copy to project, reference in custom instructions | Full support. No cross-session persistence. |
| **ChatGPT GPT** | ⭐⭐⭐ | Paste SKILL.md into Custom GPT Instructions | Works but no file references. Level 0-2 only (context limits adversarial review). |
| **Aider** | ⭐⭐½ | Place in project, reference with `--file` | Works but Aider is code-focused; general chat responses feel unnatural. |

---

## Installation

### OpenClaw (Recommended)
```bash
cp -r self-refine-skill/ ~/.openclaw/skills/
```

### Any AI Coding Tool
1. Copy `self-refine-skill/` into your project
2. Add to your system instructions: `Read and follow skills/self-refine-skill/SKILL.md`
3. Done — the AI will self-refine on every response

### ChatGPT Custom GPT
1. Open your GPT settings → Instructions
2. Paste the contents of `SKILL.md`
3. Save

---

## Academic Foundations

| Paper | Year | Key Contribution |
|-------|------|-----------------|
| Madaan et al., "Self-Refine" | 2023 | Core GENERATE→CRITIQUE→REFINE loop |
| Shinn et al., "Reflexion" | 2023 | Persistent verbal self-reflection memory |
| Dhuliawala et al., "Chain-of-Verification" | 2023 | Verification questions for factual claims |
| Kadavath et al., "Self-Calibration" | 2022 | Confidence assessment of own outputs |
| Gou et al., "CRITIC" | 2023 | Tool-interactive self-correction |
| Ng, "Agentic Design Patterns" | 2024 | Reflection as foundational agent pattern |

Full source list with URLs: [`references/sources.md`](references/sources.md)

---

## License

MIT
