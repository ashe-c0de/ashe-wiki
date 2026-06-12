# Agent Loop

## Introduction

The core characteristic of an Agent is not tool usage, memory, or planning.

The defining characteristic of an Agent is the ability to operate in a continuous loop.

Unlike a traditional LLM that generates a single response, an Agent repeatedly:

1. Observes the environment
2. Reasons about the current state
3. Decides what to do next
4. Executes an action
5. Evaluates the result

This process continues until the goal is achieved.

---

## LLM vs Agent

### LLM

A traditional LLM interaction is usually a single step.

```text
User Input
     ↓
    LLM
     ↓
 Response
```

Example:

```text
User:
What is the capital of France?

LLM:
Paris.
```

The execution ends immediately after the response is generated.

---

### Agent

An Agent continuously interacts with its environment.

```text
Goal
  ↓
Observe
  ↓
Reason
  ↓
Act
  ↓
Observe
  ↓
Reason
  ↓
Act
```

The process stops only when:

```text
Goal Achieved
```

or

```text
Task Aborted
```

---

## The Basic Agent Loop

Most Agent systems can be represented as:

```text
Observe
   ↓
Think
   ↓
Act
   ↓
Observe
```

A more detailed version:

```text
Observation
     ↓
Reasoning
     ↓
Planning
     ↓
Action
     ↓
Environment
     ↓
Observation
```

This cycle forms the foundation of modern Agent systems.

---

## Step 1: Observation

The Agent gathers information about the current state.

Sources may include:

- User requests
- Files
- Databases
- APIs
- Web pages
- Tool outputs

Example:

```text
Goal:
Fix a failing unit test
```

Observation:

```text
Test suite reports:

testCreateUser failed
```

The Agent now has context for decision making.

---

## Step 2: Reasoning

The Agent analyzes available information.

Example:

```text
The test is failing because
the expected value differs
from the actual value.
```

Reasoning is usually performed by an LLM.

The output is not yet an action.

It is a decision about what should happen next.

---

## Step 3: Planning

The Agent determines the next step.

Example:

```text
1. Open source file
2. Locate implementation
3. Compare logic
4. Modify code
5. Run tests
```

Planning can be:

### Reactive

```text
One step at a time
```

or

### Structured

```text
Generate complete plan first
```

Different Agent architectures choose different approaches.

---

## Step 4: Action

The Agent executes a task.

Examples:

```text
Read file
```

```text
Search web
```

```text
Call API
```

```text
Execute code
```

```text
Write file
```

Actions allow the Agent to interact with the outside world.

Without actions, an Agent becomes a chatbot.

---

## Step 5: Feedback

Every action produces a result.

Example:

```text
Run tests
```

Output:

```text
3 tests passed
1 test failed
```

The Agent receives new information and enters the next iteration.

```text
Observation
```

The loop continues.

---

## Example: Coding Agent

Goal:

```text
Fix failing test
```

Loop execution:

```text
Observe:
Read test failure
```

↓

```text
Reason:
Identify likely bug
```

↓

```text
Act:
Open source file
```

↓

```text
Observe:
Review implementation
```

↓

```text
Reason:
Find incorrect logic
```

↓

```text
Act:
Modify code
```

↓

```text
Act:
Run tests
```

↓

```text
Observe:
All tests pass
```

↓

```text
Goal Achieved
```

---

## Example: Research Agent

Goal:

```text
Create report about AI coding agents
```

Loop execution:

```text
Search web
```

↓

```text
Read articles
```

↓

```text
Extract information
```

↓

```text
Identify missing data
```

↓

```text
Search again
```

↓

```text
Generate report
```

↓

```text
Goal Achieved
```

The Agent repeatedly acquires new information before producing a final result.

---

## Why the Loop Matters

Without a loop:

```text
Input
 ↓
Output
```

The system cannot adapt.

With a loop:

```text
Observe
 ↓
Act
 ↓
Observe
 ↓
Act
```

The system can respond to changing conditions.

This ability is what makes Agents useful for complex tasks.

---

## Common Agent Loop Variations

### OODA Loop

Originally developed for military decision making.

```text
Observe
 ↓
Orient
 ↓
Decide
 ↓
Act
```

Many Agent systems resemble this structure.

---

### ReAct

One of the most influential Agent patterns.

```text
Thought
 ↓
Action
 ↓
Observation
```

Repeated until completion.

---

### Plan-and-Execute

```text
Create Plan
      ↓
Execute Step
      ↓
Execute Step
      ↓
Execute Step
```

Suitable for long tasks.

---

### Reflection Loop

Adds self-review.

```text
Generate
    ↓
Review
    ↓
Improve
```

Used by advanced Agent systems.

---

## Modern Agent Runtime

Most production agents implement a loop similar to:

```text
while not goal_completed:

    observe()

    think()

    choose_action()

    execute_action()

    evaluate_result()
```

This runtime loop is the heart of every Agent system.

Whether the Agent is:

- a coding assistant
- a research assistant
- a browser agent
- a multi-agent system

the same fundamental pattern exists.

---

## Key Takeaways

```text
The Agent Loop is the core mechanism of an Agent.

Agent Loop:

Observe
→ Reason
→ Plan
→ Act
→ Observe

An LLM typically generates one response.

An Agent continuously interacts with its environment.

The ability to repeatedly observe and act
is what transforms an LLM-powered system
into an Agent.
```
