# Day 04 — What Is Context Engineering?

**100 Days of Context Engineering | Phase 1: The WHY** | By Logeswaran GV (AWS Community Builder)

> *"You don't hire a 'Docker Expert' to build infrastructure. You hire a Systems Architect who knows Docker as one tool. Similarly, you don't hire an 'MCP Specialist' — you hire a Context Engineer who knows MCP as one delivery mechanism."*

---

## Explanation of Day Topic

Now that we've established the problem (Day 01), understood how LLMs process information (Day 02), and seen why the old hardcoded approach failed (Day 03), it's time to define the discipline that solves all of it.

**Context Engineering** is the complete practice of designing, building, maintaining, and optimising the flow of information into AI systems. It covers everything from how many tokens you have and what they cost, to how you retrieve relevant data, maintain agent memory, craft instructions, orchestrate multi-step workflows, and deliver context through standardised protocols.

It's a hierarchy — six distinct layers, each dependent on the ones below it. Most engineers only work on one or two layers and wonder why their AI systems underperform. This day maps the full stack.

---

## The Six-Layer Context Engineering Stack

### Layer 1 — Token Economics & Context Windows

This is the foundation. You cannot engineer context without understanding the financial and architectural reality of tokens:

- Every model has a finite context window (measured in tokens)
- Tokens cost money — both financially and in latency
- You must make strategic tradeoffs between breadth and depth of context
- Knowing when to truncate, compress, or summarise is a core CE skill

Think of it like a budget before building a house. If you don't understand your resource constraints, every decision above this layer will be compromised.

**Financial markets analogy:** Token economics is position sizing. You have a finite risk budget — you don't allocate it carelessly.

### Layer 2 — Retrieval Systems (RAG, Ranking, Re-ranking)

Because you can't fit everything into the context window, you need systems that retrieve the right information at the right time:

- **Indexing:** Making your data searchable (chunking, embedding, vector stores)
- **Retrieval:** Finding the most relevant chunks for a given query (semantic search, keyword search, hybrid)
- **Ranking:** Ordering results by relevance
- **Re-ranking:** Applying a second-pass model to improve precision before feeding context to the LLM

This is where most production AI failures actually originate. A perfect prompt with poor retrieval still fails. Bad data in, bad reasoning out.

**Financial markets analogy:** Retrieval is your market data feed. If the feed is stale, wrong, or incomplete — the trade decision downstream is compromised regardless of how sophisticated your model is.

### Layer 3 — Memory Architecture

Agents need to maintain state across multiple interactions. There are four types of memory:

- **Short-term (working memory):** The current conversation buffer
- **Episodic memory:** Records of past interactions the agent can retrieve
- **Semantic memory:** A domain knowledge store (facts, entities, relationships)
- **Procedural memory:** Learned skills and tool-use patterns

Different use cases require different memory architectures. A customer service agent needs different memory than a research assistant or a pre-trade analysis agent.

**Financial markets analogy:** Memory is your trade history and risk accounting system. Without it, every decision is made in a vacuum.

### Layer 4 — Prompt Optimisation

This is what most people call "Prompt Engineering" — but within Context Engineering, it's just the fourth layer, not the entire discipline:

- **System prompt design:** Defining the model's role, constraints, and output format
- **Few-shot examples:** Teaching by example rather than instruction alone
- **Reasoning frameworks:** Chain-of-thought, tree-of-thought, structured reasoning
- **Instruction clarity:** Removing ambiguity that leads to inconsistent behaviour

It's critical. But it's only Layer 4. Many teams over-invest here while neglecting the three layers below, which is why their agents hallucinate and underperform on live data tasks.

**Financial markets analogy:** Prompt optimisation is your trading rulebook. Rules are necessary — but rules without data don't make trades.

### Layer 5 — Agent Orchestration

When a single agent must call multiple tools, retrieve from multiple sources, and reason across complex multi-step workflows, orchestration becomes the discipline:

- **Tool selection:** How the agent decides which tool to call and when
- **Error recovery:** What happens when a tool fails or returns unexpected output
- **Loop prevention:** Detecting and breaking out of infinite reasoning loops
- **Cost optimisation:** Batching tool calls, caching results, avoiding redundant inference
- **Multi-agent coordination:** Agent A delegates to Agent B, with proper context handoff

This is where emergent intelligence appears — and where systems become fragile if poorly designed.

**Financial markets analogy:** Orchestration is your order management system. It coordinates execution across multiple venues, handles partial fills, manages timeouts, and recovers from rejections.

### Layer 6 — Protocol Delivery (MCP)

Once you've engineered all five layers below, you need a standardised way to deliver context to models and expose tools across multiple applications:

- **Consistent tool exposure:** Any client can discover and call any server's tools
- **Real-time context updates:** Live data flows into the model without manual intervention
- **Security boundaries:** Who can access which tools and data
- **Multi-client support:** One MCP server serves multiple host applications
- **Ecosystem scale:** Hundreds of tools and applications speak the same language

This is where **Model Context Protocol (MCP)** comes in. MCP is the standardised protocol for delivering engineered context at scale. It's not the discipline — it's the delivery mechanism for the discipline.

---

## Why the Layers Are Interdependent

These six layers don't operate independently. Each one depends on the layers below it:

**Layer 4 (Prompts) cannot compensate for Layer 2 (Retrieval).** A perfect system prompt with wrong data retrieved still produces wrong answers. This is the most common failure mode in production.

**Layer 5 (Orchestration) cannot compensate for Layer 3 (Memory).** Agents that can't remember across sessions make the same mistakes repeatedly and provide no continuity of intelligence.

**Layer 6 (MCP) has no value without Layers 1–5.** Delivering context via a standard protocol is meaningless if the context itself is poorly designed.

This interdependency is why Context Engineering is a **discipline** and not a collection of isolated techniques.

---

## Context Engineering vs Prompt Engineering

| Dimension | Prompt Engineering | Context Engineering |
|-----------|-------------------|---------------------|
| Scope | Single instruction | Entire information system |
| Focus | What the model is told | What the model knows |
| Timeframe | Per-request | System-wide |
| Skill set | Copywriting, instruction design | Systems architecture, data engineering, ML |
| Failure mode | Vague prompts | Stale data, poor retrieval, no memory |

**Prompt Engineering approach to a trading problem:**
*"Write me a better system prompt for the trading bot."*

**Context Engineering approach to the same problem:**
*"Let's audit the entire flow — what market data is retrievable? How fresh is it? Does the agent retain trade history across sessions? Are there edge cases in the orchestration logic? Does MCP properly expose the execution tools? Is the ranking algorithm matching trader intent against retrieved documents?"*

The second approach catches problems the first one can't even see.

---

## Why This Matters in Production

The performance frontier in AI systems has shifted over time:

- **Five years ago:** The bottleneck was model quality — better models = better results
- **Three years ago:** The bottleneck was prompt quality — better prompts = better results
- **Today:** The bottleneck is context quality — better context architecture = better results

A mediocre model with world-class context engineering consistently outperforms a state-of-the-art model with poor context engineering. This is empirically observed across organisations building production AI systems.

The discipline you're learning in this series is the current frontier of AI engineering.

---

## The Financial Markets Parallel

For professionals in financial markets infrastructure, the six-layer CE stack maps directly:

| CE Layer | Financial Markets Equivalent |
|----------|------------------------------|
| Token Economics | Position sizing and capital allocation |
| Retrieval Systems | Market data feeds and reference data |
| Memory Architecture | Trade history, risk accounting, session state |
| Prompt Optimisation | Trading rules, risk policies, constraints |
| Agent Orchestration | Order management, execution algorithms |
| Protocol Delivery (MCP) | FIX protocol, standardised exchange APIs |

Context Engineering is how you build a trading AI that doesn't lose money because it was working with the wrong information.

---

## Key Terms

| Term | What It Means |
|------|--------------|
| **Context Engineering** | The complete discipline of designing information flow for AI systems across all six architectural layers |
| **Parametric Knowledge** | Facts and patterns embedded in the model's weights during training — fixed, not retrievable |
| **Retrieval-Augmented Generation (RAG)** | The practice of fetching relevant external data before inference to provide current, specific context |
| **Memory Architecture** | The design of how an AI system stores and recalls information across interactions and sessions |
| **Agent Orchestration** | The coordination of multiple tools, memory systems, and reasoning steps in a multi-step workflow |
| **Protocol Delivery** | The standardised layer (MCP) that exposes context systems to AI models consistently and securely |

---

## What's Next

**Day 05 — The 4 Types of Context Every LLM Uses**

Not all context is equal. Tomorrow we break down the four distinct types of context that flow into any LLM — Parametric, Instructional, Conversational, and Retrieved — and explain which problems each one solves, and critically, where each one fails.

---

*#100DaysOfContextEngineering #ContextEngineering #AIEngineering #RAG #SystemsArchitecture #AWSCommunityBuilder*

[← Day 03](./Day-03-The-Old-Way-Hardcoded-Integrations.md) | [Day 05 →](./Day-05-MCP-The-Big-Idea.md)
