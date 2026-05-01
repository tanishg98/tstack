# Tanker vs AutoGen

[Microsoft AutoGen](https://github.com/microsoft/autogen) is a multi-agent conversation framework — agents talk to each other and to humans, with code execution and tool use mixed in. Tanker is a different shape: an opinionated product-build pipeline on Claude Code.

## TL;DR

- **Tanker is a product-build pipeline. AutoGen is a conversation framework.**
- **Tanker ends in a deployed URL. AutoGen ends in agent transcripts.**
- These are not competitors. AutoGen is a kit you assemble; Tanker is an opinionated end-to-end flow.

---

## What you get with Tanker that you don't get with AutoGen

### 1. **A finished, opinionated pipeline**

`/cto "<brief>"` runs a complete sequence: intake → context → reference → grill → benchmark → PRD → architect → plan → provision → build → mvp review → deploy → monitor. You don't assemble agents and a chat protocol; you submit a brief and walk through two human gates.

AutoGen gives you `ConversableAgent`, group chat, and the building blocks. The pipeline is your job.

### 2. **Real infrastructure provisioning**

GitHub repo + Supabase project + Vercel project + Railway service, all created via official APIs from a vault. AutoGen has no equivalent — provisioning is on you.

### 3. **Two human gates pre-qualified by review agents**

Tanker stops at exactly two points (PRD, MVP), each gated by a review agent before it reaches the human. AutoGen has `human_input_mode` (ALWAYS / TERMINATE / NEVER) at the conversation layer, but no built-in gate-with-pre-qualification pattern.

### 4. **34 specialized skills + 9 agents shipped**

Tanker comes with the surface already built: skills for every phase of a build, agents for review and provisioning, always-on rules for code quality. AutoGen is a framework; the skills are yours to write.

### 5. **Cost ceiling enforced at the framework level**

`--max-cost-usd` halts the run at the cap. AutoGen has token-usage callbacks; the cap is yours to wire.

### 6. **Local semantic retrieval over your corpus**

brain-index + refs-index, ChromaDB-backed, all local. AutoGen has no built-in retrieval layer — bring your own.

### 7. **Claude Code-native**

Tanker is a `.claude/` folder. Drop it in any project, slash commands appear instantly. AutoGen is a Python framework you import.

---

## Side-by-side

| | AutoGen | Tanker |
|---|---|---|
| **Primary abstraction** | `ConversableAgent` + group chat | Skill + Agent + Rule |
| **Programming model** | Python API | Claude Code slash commands |
| **Agent communication** | Conversational message-passing | Typed Message envelope (`messages.jsonl`) + `state.json` |
| **Code execution** | `LocalCommandLineCodeExecutor` | Bash tool (Claude Code native) |
| **Human input** | `human_input_mode` enum at conversation layer | Two mandatory review gates pre-qualified by agents |
| **Multi-LLM** | Yes — OpenAI, Azure, local, etc. | Claude Code-native |
| **Provisioning** | You wire it | `gh + supabase + vercel + railway` provisioner agents |
| **Quality rails** | Your prompts | Always-on rules: builder-ethos, code-standards, static-site-standards |
| **Use case** | Custom agentic apps | Product builds |

---

## When AutoGen is the better choice

- You're building a **custom agentic application** — research workflow, synthetic data generation, conversation-heavy automation.
- You need **multi-provider** support (OpenAI, Azure, local).
- You're working **outside Claude Code**.
- You want a **kit to assemble**, not an opinionated pipeline.

## When Tanker is the better choice

- You're on **Claude Code** and want to ship products faster.
- You want **provisioning + deploy + monitoring** built in.
- You want **human gates** at the highest-leverage decisions.
- You want **opinionated quality rails** baked in (No AI Slop ban list, semantic HTML, mobile-first).

---

Both MIT. Different shapes for different needs.
