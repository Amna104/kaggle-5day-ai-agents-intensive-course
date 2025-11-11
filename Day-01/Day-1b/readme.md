# Day 1b: Multi-Agent Systems & Workflow Patterns 🚀

## Overview
This is my learning from Day 1b of the **Kaggle 5-Day AI Agents Course**. Building on the single-agent foundation from Day 1a, I learned how to architect **multi-agent systems** where specialized agents collaborate like a team to solve complex problems.

## Key Concepts Learned

### Why Multi-Agent Systems?

**The Problem: The "Do-It-All" Agent**
- Single monolithic agents trying to handle everything become:
  - Hard to debug (which part failed?)
  - Difficult to maintain
  - Unreliable with complex tasks
  - Confusing with long instruction prompts

**The Solution: Team of Specialists**
- Multiple simple, specialized agents that collaborate
- Each agent has ONE clear job
- Easier to build, test, and maintain
- More powerful and reliable when working together

### Single Agent vs Multi-Agent Architecture

**Single Agent:**
```
One Agent → Does Everything (Research + Write + Edit + Fact-check)
```

**Multi-Agent Team:**
```
Root Coordinator
├── Research Specialist
├── Writer Specialist
└── Editor Specialist
```

## Four Workflow Patterns Mastered

### 1. LLM-Based Orchestration (Dynamic)
**When to Use:** When the LLM should decide what to do dynamically

**Example Built:** Research & Summarization System
- **Research Agent**: Uses Google Search to find information
- **Summarizer Agent**: Creates concise summaries
- **Root Coordinator**: Orchestrates the workflow using instructions

**Key Feature:** LLM decides which agents to call and when

```python
root_agent = Agent(
    name="ResearchCoordinator",
    instruction="First call ResearchAgent, then SummarizerAgent",
    tools=[AgentTool(research_agent), AgentTool(summarizer_agent)]
)
```

**Limitation:** Can be unpredictable - LLM might skip steps or run them out of order

---

### 2. Sequential Workflow (Assembly Line)
**When to Use:** When order matters and tasks build on each other

**Example Built:** Blog Post Creation Pipeline
1. **Outline Agent** → Creates blog structure
2. **Writer Agent** → Writes full blog post
3. **Editor Agent** → Polishes and edits

**Key Feature:** Guaranteed step-by-step execution in exact order

```python
root_agent = SequentialAgent(
    name="BlogPipeline",
    sub_agents=[outline_agent, writer_agent, editor_agent]
)
```

**Perfect for:** Linear pipelines where each step depends on the previous one

---

### 3. Parallel Workflow (Concurrent Execution)
**When to Use:** When tasks are independent and speed matters

**Example Built:** Multi-Topic Research System
- **Tech Researcher** → AI/ML trends (runs concurrently)
- **Health Researcher** → Medical breakthroughs (runs concurrently)
- **Finance Researcher** → Fintech trends (runs concurrently)
- **Aggregator Agent** → Combines all findings (runs after parallel tasks)

**Key Feature:** All agents run simultaneously, dramatically speeding up workflow

```python
parallel_team = ParallelAgent(
    sub_agents=[tech_researcher, health_researcher, finance_researcher]
)

root_agent = SequentialAgent(
    sub_agents=[parallel_team, aggregator_agent]
)
```

**Perfect for:** Independent tasks that don't depend on each other

---

### 4. Loop Workflow (Iterative Refinement)
**When to Use:** When output needs multiple rounds of improvement

**Example Built:** Story Writing & Critique Loop
1. **Initial Writer Agent** → Creates first draft
2. **Loop begins:**
   - **Critic Agent** → Reviews story, provides feedback or "APPROVED"
   - **Refiner Agent** → Either rewrites story OR calls `exit_loop()` function
3. Loop continues until approved or max iterations reached

**Key Feature:** Repeated cycles of review and refinement

```python
story_loop = LoopAgent(
    sub_agents=[critic_agent, refiner_agent],
    max_iterations=2
)

root_agent = SequentialAgent(
    sub_agents=[initial_writer_agent, story_loop]
)
```

**Perfect for:** Quality control and iterative improvement tasks

---

## Critical Concepts Mastered

### 1. Agent Communication via State
Agents share data through **session state** using `output_key`:

```python
research_agent = Agent(
    output_key="research_findings"
)

summarizer_agent = Agent(
    instruction="Summarize this: {research_findings}"
)
```

### 2. AgentTool Wrapping
Convert sub-agents into tools that parent agents can call:

```python
tools=[AgentTool(research_agent), AgentTool(summarizer_agent)]
```

### 3. FunctionTool for Custom Logic
Wrap Python functions to give agents custom capabilities:

```python
def exit_loop():
    return {"status": "approved"}

refiner_agent = Agent(
    tools=[FunctionTool(exit_loop)]
)
```

### 4. Nesting Workflows
Combine workflow patterns for sophisticated systems:

```python
# Parallel research, then sequential aggregation
root_agent = SequentialAgent(
    sub_agents=[
        ParallelAgent(sub_agents=[...]),
        aggregator_agent
    ]
)
```

## Decision Tree: Choosing the Right Pattern

```
What kind of workflow do you need?

├─ Fixed Pipeline (A → B → C)
│  └─ Use SequentialAgent
│
├─ Concurrent Tasks (A, B, C all at once)
│  └─ Use ParallelAgent
│
├─ Iterative Refinement (A ⇄ B)
│  └─ Use LoopAgent
│
└─ Dynamic Decisions (Let LLM decide)
   └─ Use LLM Orchestrator (Agent with sub-agents as tools)
```

## Quick Reference Table

| Pattern | When to Use | Example Use Case | Key Feature |
|---------|-------------|------------------|-------------|
| **LLM-based** | Dynamic orchestration | Research + Summarize | LLM decides what to call |
| **Sequential** | Order matters | Outline → Write → Edit | Deterministic order |
| **Parallel** | Independent tasks | Multi-topic research | Concurrent execution |
| **Loop** | Needs refinement | Writer + Critic | Repeated cycles |

## Practical Applications Built

### 1. Research & Summarization System
- Dynamically searches and summarizes information
- **Use case:** Automated research briefs

### 2. Blog Post Creation Pipeline
- Creates outline → writes draft → edits final version
- **Use case:** Content creation automation

### 3. Multi-Topic Research Dashboard
- Researches tech, health, and finance simultaneously
- **Use case:** Executive briefings, market intelligence

### 4. Iterative Story Refinement
- Writes story → receives critique → refines → repeats
- **Use case:** Creative writing, quality assurance

## Key Takeaways

1. **Specialization > Monolithic**: Multiple specialized agents are better than one complex agent

2. **Choose the Right Pattern**: Each workflow pattern has specific use cases
   - Sequential for pipelines
   - Parallel for speed
   - Loop for quality
   - LLM-based for flexibility

3. **State Management**: Agents communicate through `output_key` and state placeholders like `{research_findings}`

4. **Composability**: Patterns can be nested and combined for complex workflows

5. **Debugging Advantage**: When something fails, you know exactly which specialist agent had the issue

## Technical Skills Gained

- ✅ Building multi-agent systems with ADK
- ✅ Implementing Sequential, Parallel, and Loop workflows
- ✅ Using `AgentTool` to wrap sub-agents
- ✅ Using `FunctionTool` to add custom Python functions
- ✅ Managing state between agents with `output_key`
- ✅ Nesting workflow patterns for complex systems
- ✅ Designing exit conditions for loop agents

## Resources

- [ADK Agents Documentation](https://google.github.io/adk-docs/agents/)
- [Sequential Agents](https://google.github.io/adk-docs/agents/workflow-agents/sequential-agents/)
- [Parallel Agents](https://google.github.io/adk-docs/agents/workflow-agents/parallel-agents/)
- [Loop Agents](https://google.github.io/adk-docs/agents/workflow-agents/loop-agents/)
- [Custom Agents](https://google.github.io/adk-docs/agents/custom-agents/)

## Next Steps
Moving forward to Day 2 to learn about **Custom Functions, MCP Tools, and Long-Running Operations**.

---

**Course**: Kaggle 5-Day AI Agents  
**Completion Date**: November 11, 2025  