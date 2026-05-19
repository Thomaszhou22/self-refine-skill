# No-Fluff Skill — Reference Sources

## Official Guides

1. **OpenAI Prompt Engineering Guide**
   - platform.openai.com/docs/guides/prompt-engineering
   - Key: Use developer/system messages for high-level behavioral control; calibrate model behavior through instructions; different model types need different prompting strategies

2. **Anthropic Claude Prompting Best Practices**
   - platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
   - Key: Claude calibrates response length to task complexity; to reduce verbosity add "Provide concise, focused responses. Skip non-essential context"; positive examples > negative instructions; more literal instruction following in newer models

3. **Microsoft Azure OpenAI Prompt Engineering**
   - learn.microsoft.com/en-us/azure/foundry/openai/concepts/prompt-engineering
   - Key: Instructions are the most important prompt component; system/user/assistant roles provide structure; prompt construction is iterative

4. **Google Gemini Prompting Strategies**
   - ai.google.dev/gemini-api/docs/prompting-strategies
   - (Redirect issues prevented full read, but general principles apply)

## Academic Papers

5. **Self-Refine: Iterative Refinement with Self-Feedback** (Madaan et al., 2023)
   - arxiv.org/abs/2303.17651
   - Key: Generate → Feedback → Refine loop; up to 4 iterations; no supervised data needed; GPT-4 improved 8.7 units on code optimization

6. **Chain-of-Thought Prompting Elicits Reasoning in LLMs** (Wei et al., 2022)
   - arxiv.org/abs/2201.11903
   - Key: Breaking complex tasks into step-by-step reasoning improves accuracy; intermediate reasoning steps expose logic for verification

7. **Reflexion: Autonomous Agent with Dynamic Memory and Self-Reflection**
   - promptengineering.org/reflexion
   - Key: Agents emulate human self-reflection; dynamic memory enables learning from mistakes

8. **Self-Consistency Improves Chain of Thought Reasoning** (Wang et al., 2022)
   - Key: Running multiple CoT paths and taking majority vote improves reliability

## Expert Insights

9. **Andrew Ng's Agentic Design Patterns**
   - learn.deeplearning.ai/courses/agentic-ai
   - Key: 4 patterns — Reflection, Tool Use, Planning, Multi-Agent Collaboration; Reflection is the foundational pattern for self-improvement

10. **Paul Graham — "Write Simply"** (paulgraham.com/simply.html)
    - Key: Simple writing lets readers focus on ideas, not prose; simple writing keeps you honest ("If you say nothing simply, it will be obvious to everyone"); edit by cutting, not adding

11. **Paul Graham — "Writing, Briefly"** (paulgraham.com/writing44.html)
    - Key: Writing well = thinking well; brevity is a feature

12. **Ethan Mollick — "Co-Intelligence"**
    - Key: AI works best as a co-intelligence partner; prompt quality directly determines output quality; iteration and refinement are essential

## Technical Writing

13. **Google Technical Writing: Be Concise**
    - developers.google.com/tech-writing/error-messages/be-concise
    - Key: Eliminate unnecessary text; use active voice; be concise without sacrificing clarity; don't make things so brief they become cryptic

14. **Google Developer Documentation Style Guide**
    - developers.google.com/style
    - Key: Clear, consistent technical documentation; editorial standards for developer audiences

## Hallucination Prevention

15. **Anthropic: Reduce Hallucinations**
    - platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations
    - Key: Allow "I don't know"; use direct quotes for grounding; verify with citations; chain-of-thought verification; external knowledge restriction

16. **Morphik: 7 Proven Methods to Eliminate AI Hallucinations**
    - morphik.ai/blog/eliminate-hallucinations-guide
    - Key: Properly implemented safeguards achieve 96% hallucination reduction; layered defenses > silver bullets; chain-of-thought improves accuracy by 35%

17. **Reddit/PromptEngineering: 12 Tested Techniques Against Hallucinations**
    - Key: "Explain only core, well-established aspects"; eliminated 89% of fake claims; restrict to established knowledge

## Prompt Engineering Best Practices

18. **Braintrust: Systematic Prompt Engineering**
    - braintrust.dev/articles/systematic-prompt-engineering
    - Key: Treat prompt engineering as systems engineering, not creative writing; modular prompt architecture; clear requirements definition; version control prompts

19. **Lakera: Ultimate Guide to Prompt Engineering 2026**
    - lakera.ai/blog/prompt-engineering-guide
    - Key: Structured, detailed prompts produce accurate, actionable outputs; structured prompts reduce hallucinations

20. **LearnPrompting.org: Self-Refine**
    - learnprompting.org/docs/advanced/self_criticism/self_refine
    - Key: 3-step approach (generate → feedback → refine); iterative until satisfactory; works across code optimization, readability, sentiment analysis

21. **Aakash Gupta: Prompt Engineering in 2025**
    - news.aakashg.com/p/prompt-engineering
    - Key: Research-backed techniques from production AI companies; latest frameworks for product teams

## GitHub Repositories

22. **Awesome-Prompt-Engineering** (promptslab)
    - github.com/promptslab/Awesome-Prompt-Engineering
    - Key: Curated resources for prompt engineering with GPT/ChatGPT/PaLM

23. **awesome-gpt-prompt-engineering** (snwfdhmp)
    - github.com/snwfdhmp/awesome-gpt-prompt-engineering
    - Key: Curated list of resources and tools for LLM prompt engineering
