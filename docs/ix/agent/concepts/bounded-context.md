# Bounded Context in Agents

## Core Idea

An Agent does not operate on unlimited knowledge or infinite state.

Instead, it operates within a **bounded context**:

```text
Context = What the Agent currently knows + can access + can reason over
```

This boundary is one of the most important constraints in real-world agent systems.

---

## What is a Bounded Context?

A bounded context is:

> The limited slice of information an Agent can use at any given moment to make decisions.

It includes:

- current conversation history
- retrieved documents (RAG)
- tool outputs
- memory snapshots
- system prompts
- environment state

---

## Why “Bounded” Matters

Even though models feel powerful, they are still constrained by:

### 1. Context Window

```text
Finite tokens → finite memory
```

Example:

```text
8K / 32K / 128K tokens limit
```

The Agent cannot see everything at once.

---

### 2. Tool Latency

Not all information is immediately available.

```text
Need to search → wait → observe
```

---

### 3. Relevance Filtering

More context ≠ better decisions.

Too much information causes:

- distraction
- noise
- hallucinated reasoning
- slower planning

---

## Bounded Context vs Memory

They are related but not identical.

### Memory

```text
Persistent storage across time
```

Examples:

- vector database
- chat history
- long-term facts

---

### Bounded Context

```text
Active working set used right now
```

It is a subset of memory.

---

## Agent Context Structure

A typical agent context looks like:

```text
┌──────────────────────────────┐
│        Bounded Context       │
├──────────────────────────────┤
│ System Instructions          │
│ User Goal                    │
│ Conversation History         │
│ Retrieved Knowledge (RAG)    │
│ Tool Outputs                 │
│ Working Memory State         │
└──────────────────────────────┘
```

Everything the Agent uses for reasoning comes from here.

---

## Why Agents Need Context Management

Without context control:

```text
Everything is thrown into prompt
```

This leads to:

- context overflow
- degraded reasoning
- high cost
- unstable behavior

---

## Context as a Working Set

Think of bounded context like CPU memory:

```text
Disk (Long-term memory)
   ↓
RAM (Bounded context)
   ↓
CPU (Reasoning step)
```

The Agent only operates on what is in "RAM".

---

## Context Construction Pipeline

In real systems:

```text
User Input
   ↓
Retrieve Memory
   ↓
Call Tools
   ↓
Fetch External Data
   ↓
Assemble Context
   ↓
LLM Reasoning
```

This assembly step is critical.

---

## Context Selection Problem

The hardest problem is not generation.

It is:

> What should be included in the context?

Too little context:

- missing information
- wrong decisions

Too much context:

- noise
- token overflow
- poor reasoning focus

---

## Example: Coding Agent

Goal:

```text
Fix failing test
```

Bounded context may include:

```text
- test output
- relevant source file
- recent git diff
- previous attempts
```

But NOT:

```text
- entire repository
- unrelated modules
- old logs
```

---

## Example: Research Agent

Goal:

```text
Write report on AI agents
```

Bounded context:

```text
- top retrieved papers
- summaries of articles
- user constraints
```

Not included:

```text
- all web pages ever visited
- irrelevant search results
```

---

## Context Window vs Bounded Context

These are different:

### Context Window

```text
Hard model limit
```

### Bounded Context

```text
System design choice
```

You can have:

- large context window but poor selection
- small context window but excellent selection

Selection quality matters more.

---

## Context Engineering (Modern Term)

Modern agent systems increasingly focus on:

```text
Context Engineering > Prompt Engineering
```

It includes:

- retrieval strategy
- memory ranking
- summarization
- compression
- tool filtering

---

## Context Compression

To fit within limits:

### Summarization

```text
Long history → short summary
```

---

### Filtering

```text
Keep only relevant info
```

---

### Abstraction

```text
Details → high-level representation
```

---

## Failure Modes

### 1. Context Flooding

Too much irrelevant data:

```text
Agent confused
```

---

### 2. Context Loss

Important information dropped:

```text
Agent forgets constraints
```

---

### 3. Stale Context

Old information remains:

```text
Agent uses outdated facts
```

---

## Relation to Agent Loop

Bounded context is updated every loop:

```text
Observe
  ↓
Update Context
  ↓
Reason
  ↓
Act
  ↓
New Observation
  ↓
Update Context
```

So context is not static—it evolves.

---

## Key Insight

```text
An Agent is not just a reasoning system.

It is a system that continuously constructs
and reconstructs its bounded context.
```

---

## Design Principle

Good agent design:

```text
Optimize context quality,
not context size.
```

---

## Key Takeaways

```text
Bounded context = active working state of an Agent

It is:
- dynamic
- limited
- continuously updated

Agent performance depends heavily on:
- what enters the context
- what is excluded
- how information is compressed

Context is not passive data.

It is the Agent’s working mind state.
```
