<div align="center">

# 🪞 Self-Refine Skill

**AI 智能体的系统性自我反思。**

*让每一次 AI 回复都更好——在用户看到之前。*

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

**[English](README.md)** | **中文**

</div>

---

## 它做什么

大多数 AI 回复都是初稿。这个 Skill 在每次回复前加入一个**不可见的自我审查步骤**：

```
生成 → 批评 → 精炼 → 检查 → 交付
```

基于同行评审研究：迭代式自我精炼可带来**约 20% 的质量提升**（Madaan et al., 2023）。

**一个文件。零依赖。零 API 成本。** 宿主 AI 自己当自己的批评者。

---

## 实测效果

在 **5 个类别的 20 个问题**上基准测试，采用受控的前后对比评估。→ [完整报告](BENCHMARK.md)

| 指标 | 结果 |
|--------|--------|
| **整体提升** | **+21.2%** |
| 得到改善的问题 | 20/20（100%） |
| 单题最大提升 | +43.9% |

**各维度提升：**

| 维度 | Δ |
|-----------|---|
| 简洁性 | +27.7% |
| 可执行性 | +25.0% |
| 回答完整性 | +24.8% |
| 逻辑完整性 | +23.7% |
| 事实准确性 | +14.8% |
| 内部一致性 | +11.9% |

📖 [阅读完整基准报告 →](BENCHMARK.md)

---

## 优化前后对比

<details>
<summary><b>示例 1 — 代码调试</b></summary>

**用户：** *"我的 React 应用页面加载即崩溃。错误：`Cannot read properties of undefined (reading 'map')`"*

**❌ 无 Self-Refine：**
> 这个错误通常发生在你对 undefined 值调用 `.map()` 时。在 React 里，一般是你想渲染列表但数据还没加载。你应该在 map 之前检查变量是否有定义。可以用可选链或条件判断。示例如下：
> ```jsx
> {data && data.map(item => <div key={item.id}>{item.name}</div>)}
> ```
> 另外，确保 state 初始化正确……

**✅ 有 Self-Refine：**
> 崩溃原因是 `undefined` 上调 `.map()`。两种修法：
>
> **快速修复：**
> ```jsx
> {data?.map(item => <div key={item.id}>{item.name}</div>)}
> ```
> **根因修复：**
> ```jsx
> const [data, setData] = useState([]);  // 不要 useState() 或 null
> ```
> 大概率 `useState()` 返回 `undefined` → `.map()` 抛错。

</details>

<details>
<summary><b>示例 2 — 架构决策</b></summary>

**用户：** *"会话存储用 Redis 还是 PostgreSQL？"*

**❌ 无 Self-Refine：**
> 两者都是好选择，取决于你的需求。Redis 快、内存型。PostgreSQL 持久但慢些。会话一般首选 Redis。但如果你已经有 PostgreSQL，用它也行。考虑速度、持久性和复杂度。

**✅ 有 Self-Refine：**
> **Redis。** 理由：
>
> | | Redis | PostgreSQL |
> |---|---|---|
> | 延迟 | ~0.1ms | ~2-5ms |
> | TTL | 内置 | 手动清理 |
> | 扩展 | 百万级、无 schema 开销 | 行膨胀、VACUUM |
>
> 只有用户 <100 且不想引入 Redis 时才用 PostgreSQL。
>
> ```bash
> npm install connect-redis express-session
> ```

</details>

<details>
<summary><b>示例 3 — 简洁解释</b></summary>

**用户：** *"用 3 句话解释 JWT。"*

**❌ 无 Self-Refine：**
> JWT（JSON Web Token）是 Web 应用中处理认证的流行方法。它通过创建一个包含用户信息的签名令牌来工作，随每个请求发送。服务器无需存储会话数据即可验证令牌，因此是无状态且可扩展的。JWT 由三部分组成：头部、载荷和签名，全部以 Base64 编码……

**✅ 有 Self-Refine：**
> JWT 是一个签名 JSON 令牌（`header.payload.signature`），用于证明身份。登录后服务器签发；客户端在每个请求的 `Authorization: Bearer <token>` 中携带。服务器验证签名——无需会话存储。

</details>

---

## 工作原理

### 6 个反思维度

| # | 维度 | 捕捉什么 |
|---|-----------|---------|
| 1 | **逻辑完整性** | 断裂的推理链 |
| 2 | **事实准确性** | 未经核实的断言 |
| 3 | **回答完整性** | 被忽略的子问题 |
| 4 | **简洁性** | 冗余、废话 |
| 5 | **可执行性** | 没有步骤的空泛建议 |
| 6 | **内部一致性** | 自相矛盾 |

### 4 个深度级别（自动选择）

| 级别 | 适用场景 | 轮数 | Token 成本 |
|:-----:|------|:------:|:----------:|
| **0** | 简单问答 | 0 | +0% |
| **1** | 大多数对话 | 1 | ~15% |
| **2** | 复杂技术问题 | 2 | ~30% |
| **3** | 高风险 / 对抗性 | 3 | ~50% |

无需配置。智能体根据任务复杂度自动选择合适的深度。

### 跨会话学习（Reflexion）

智能体在自我审查中发现反复出现的错误模式时，会把笔记存入记忆。下次会话避开同样的错误。基于 Shinn et al. (2023)。

---

## 安装

<details>
<summary><b>OpenClaw（推荐）</b></summary>

```bash
# 复制到 skills 目录
cp -r self-refine-skill/ ~/.openclaw/skills/
```

完成。OpenClaw 自动检测 skill。

</details>

<details>
<summary><b>Claude Code</b></summary>

```bash
# 复制到项目
cp -r self-refine-skill/ skills/

# 添加到 CLAUDE.md
echo "Read and follow skills/self-refine-skill/SKILL.md" >> CLAUDE.md
```

</details>

<details>
<summary><b>Cursor</b></summary>

```bash
# 复制到项目根目录
cp -r self-refine-skill/ skills/

# 添加到 .cursorrules
echo "Read and follow skills/self-refine-skill/SKILL.md" >> .cursorrules
```

</details>

<details>
<summary><b>Gemini CLI</b></summary>

```bash
# 复制到项目，添加到 GEMINI.md
cp -r self-refine-skill/ skills/
echo "Read and follow skills/self-refine-skill/SKILL.md" >> GEMINI.md
```

</details>

<details>
<summary><b>GitHub Copilot</b></summary>

```bash
# 添加到仓库的 Copilot 指令
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
<summary><b>Cline / AI 编程助手</b></summary>

把 `self-refine-skill/` 复制进项目，在助手的自定义指令中引用 `SKILL.md`。

</details>

<details>
<summary><b>JetBrains AI / Junie</b></summary>

1. 打开 Settings → Tools → AI Assistant → System Instructions
2. 添加：`Read and follow skills/self-refine-skill/SKILL.md`
3. 把 skill 文件夹放在项目根目录

</details>

<details>
<summary><b>Zed</b></summary>

1. 打开 Zed 设置
2. 添加到 context 或 assistant 指令：
```
Read and follow skills/self-refine-skill/SKILL.md
```

</details>

<details>
<summary><b>Kiro</b></summary>

1. 把 skill 文件夹加入项目
2. 在 Kiro 的指令配置中引用

</details>

<details>
<summary><b>OpenCode</b></summary>

1. 把 skill 文件夹加入项目
2. 添加到 `AGENTS.md`：`Read and follow skills/self-refine-skill/SKILL.md`

</details>

<details>
<summary><b>ChatGPT Custom GPT</b></summary>

1. 打开你的 GPT → Settings → Instructions
2. 粘贴 `SKILL.md` 的内容
3. 保存

**注意：** SKILL.md 内置了无文件访问环境的内联模板——所有深度级别都可用。反思记忆仅对话内有效（无持久化）。

</details>

<details>
<summary><b>任意 AI 工具</b></summary>

把 `SKILL.md` 复制进你的系统提示或指令文件。只需要这一个文件。

</details>

---

## 平台兼容性

**热门平台（按使用量排序）：**

| 平台 | 评级 | Reflexion 记忆 | 说明 |
|----------|:------:|:----------------:|-------|
| **Claude Code** | ⭐⭐⭐⭐⭐ | ✅ 文件写入 | 2026 年第一编程智能体，完整支持 |
| **Cursor** | ⭐⭐⭐⭐⭐ | ✅ 文件写入 | $2B ARR 的 AI IDE，完整支持 |
| **GitHub Copilot** | ⭐⭐⭐⭐⭐ | ✅ 文件写入 | 用户量最大，完整支持 |
| **Codex CLI** | ⭐⭐⭐⭐⭐ | ✅ 文件写入 | OpenAI 编程智能体，完整支持 |
| **ChatGPT** | ⭐⭐⭐⭐ | ⚠️ 仅对话内 | 内联模板自动加载，全级别可用 |

<details>
<summary><b>展开全部 14 个支持平台</b></summary>

| 平台 | 评级 | Reflexion 记忆 | 安装 |
|----------|:------:|:----------------:|---------|
| **OpenClaw** | ⭐⭐⭐⭐⭐ | ✅ `memory/` 目录 | 复制到 `skills/`，自动检测 |
| **Claude Code** | ⭐⭐⭐⭐⭐ | ✅ 文件写入 | 复制进项目，加到 `CLAUDE.md` |
| **Cursor** | ⭐⭐⭐⭐⭐ | ✅ 文件写入 | 复制进项目，加到 `.cursorrules` |
| **GitHub Copilot** | ⭐⭐⭐⭐⭐ | ✅ 文件写入 | 加到 `.github/copilot-instructions.md` |
| **Codex CLI** | ⭐⭐⭐⭐⭐ | ✅ 文件写入 | 加到 `AGENTS.md` |
| **Gemini CLI** | ⭐⭐⭐⭐⭐ | ✅ 文件写入 | 加到 `GEMINI.md` |
| **Windsurf** | ⭐⭐⭐⭐⭐ | ✅ 文件写入 | 加到 `.windsurfrules` |
| **Cline** | ⭐⭐⭐⭐⭐ | ✅ 文件写入 | 加到自定义指令 |
| **JetBrains AI / Junie** | ⭐⭐⭐⭐½ | ✅ 文件写入 | 加到 AI Assistant 指令 |
| **Aider** | ⭐⭐⭐⭐ | ✅ 文件写入 | 放进项目，用 `--file` 引用 |
| **Zed** | ⭐⭐⭐⭐ | ✅ 文件写入 | 加到 Zed 设置 |
| **ChatGPT GPT** | ⭐⭐⭐⭐ | ⚠️ 仅对话内 | 把 SKILL.md 粘贴进 GPT Instructions |
| **Kiro** | ⭐⭐⭐⭐ | ✅ 文件写入 | 加到 Kiro 指令 |
| **OpenCode** | ⭐⭐⭐⭐ | ✅ 文件写入 | 加到 AGENTS.md |

</details>

---

## 学术基础

| 论文 | 年份 | 我们用了什么 |
|-------|:----:|-------------|
| **Self-Refine** — Madaan et al. | 2023 | 核心的生成→批评→精炼循环 |
| **Reflexion** — Shinn et al. | 2023 | 持久化的自我反思记忆 |
| **Chain-of-Verification** — Dhuliawala et al. | 2023 | 事实核查验证问题 |
| **Self-Calibration** — Kadavath et al. | 2022 | 置信度评估 |
| **CRITIC** — Gou et al. | 2023 | 工具交互式自我纠正 |
| **Agentic Design Patterns** — Andrew Ng | 2024 | 反思作为基础模式 |

完整文献：[`references/sources.md`](references/sources.md)

---

## 文件结构

```
self-refine-skill/
├── SKILL.md                          # 核心指令（唯一必需文件）
├── README.md                         # 英文说明（本文件为中文版）
├── LICENSE                           # MIT
└── references/
    ├── sources.md                    # 学术来源与链接
    └── reflection-templates.md       # 按深度级别即用的内部提示模板
```

---

<div align="center">

**好答案如好代码：没有多余的行，每一行都有用途。**

如果这个 Skill 提升了你 AI 的输出质量，欢迎点个 ⭐。

</div>
