# Day 1: From Prompt to Action 🚀

## Overview
This is my learning from Day 1 of the **Kaggle 5-Day AI Agents Course**. On this day, I built my first AI agent using Google's Agent Development Kit (ADK) and learned the fundamental difference between a simple LLM and an AI agent.

## Key Concepts Learned

### What is an AI Agent?
I learned that AI agents go beyond simple prompt-response interactions:

**Traditional LLM:**
```
Prompt → LLM → Text Response
```

**AI Agent:**
```
Prompt → Agent → Thought → Action → Observation → Final Answer
```

The key difference is that agents can **take actions** to gather information and accomplish tasks, not just generate text responses.

## What I Built

### 1. First AI Agent
Created a simple agent using ADK with the following components:
- **Model**: Gemini 2.5 Flash Lite
- **Tool**: Google Search capability
- **Purpose**: Answer general questions using current information

### 2. Agent Configuration
Learned to configure agents with:
- **Name & Description**: Identity and purpose
- **Model**: The LLM powering the agent's reasoning
- **Instructions**: Guiding prompt for agent behavior
- **Tools**: Actions the agent can perform (e.g., Google Search)

## Technical Implementation

### Setup Steps
1. Installed Agent Development Kit (ADK)
2. Configured Gemini API key
3. Imported necessary ADK components
4. Set up retry configuration for handling API errors

### Code Structure
```python
from google.adk.agents import Agent
from google.adk.models.google_llm import Gemini
from google.adk.runners import InMemoryRunner
from google.adk.tools import google_search

# Define the agent
root_agent = Agent(
    name="helpful_assistant",
    model=Gemini(model="gemini-2.5-flash-lite"),
    description="A simple agent that can answer general questions.",
    instruction="You are a helpful assistant. Use Google Search for current info.",
    tools=[google_search]
)

# Create a runner
runner = InMemoryRunner(agent=root_agent)

# Run the agent
response = await runner.run_debug("Your question here")
```

## Key Takeaways

1. **Agents vs LLMs**: Agents can reason about when to use tools and take actions, making them more powerful than simple text generators

2. **Tool Usage**: The agent automatically decides when to use Google Search based on:
   - Available tools it has access to
   - Instructions provided to it
   - Nature of the question asked

3. **ADK Components**:
   - `Agent`: The core component defining behavior
   - `Runner`: Orchestrates conversation and manages responses
   - `Tools`: External capabilities the agent can use

4. **ADK Web UI**: A built-in interface for testing, debugging, and visualizing agent behavior

## Practical Applications

The agent I built can:
- ✅ Answer questions requiring current information
- ✅ Search the web when needed
- ✅ Provide up-to-date responses
- ✅ Handle queries about weather, news, events, etc.

## Example Queries Tested
- "What is Agent Development Kit from Google?"
- "What's the weather in London?"
- Questions about current events and recent information

## Tools & Technologies

- **ADK (Agent Development Kit)**: Google's framework for building AI agents
- **Gemini API**: Google's generative AI model
- **Python**: Primary programming language
- **Kaggle Notebooks**: Development environment

## Next Steps
Moving forward to Day 2 to learn about **architecting multi-agent systems** where multiple agents work together to solve complex problems.

---

**Course**: Kaggle 5-Day AI Agents  
**Completion Date**: November 11, 2025  