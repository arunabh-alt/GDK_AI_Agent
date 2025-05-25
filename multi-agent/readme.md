# Multi-Agent Systems in ADK

This example demonstrates how to create a multi-agent system in ADK, where specialized agents collaborate to handle complex tasks, each focusing on their area of expertise.

## What is a Multi-Agent System?

A Multi-Agent System is an advanced pattern in the Agent Development Kit (ADK) that allows multiple specialized agents to work together to handle complex tasks. Each agent can focus on a specific domain or functionality, and they can collaborate through delegation and communication to solve problems that would be difficult for a single agent.

## Project Structure Requirements

For multi-agent systems to work properly with ADK, your project must follow a specific structure:

```
parent_folder/
├── root_agent_folder/           # Main agent package (e.g., "manager")
│   ├── __init__.py              # Must import agent.py
│   ├── agent.py                 # Must define root_agent
│   ├── .env                     # Environment variables
│   └── sub_agents/              # Directory for all sub-agents
│       ├── __init__.py          # Empty or imports sub-agents
│       ├── agent_1_folder/      # Sub-agent package
│       │   ├── __init__.py      # Must import agent.py
│       │   └── agent.py         # Must define an agent variable
│       ├── agent_2_folder/
│       │   ├── __init__.py
│       │   └── agent.py
│       └── ...
```

### Essential Structure Components:

1. **Root Agent Package**
   - Must have the standard agent structure (like in the basic agent example)
   - The `agent.py` file must define a `root_agent` variable

2. **Sub-agents Directory**
   - Typically organized as a directory called `sub_agents` inside the root agent folder
   - Each sub-agent should be in its own directory following the same structure as regular agents

3. **Importing Sub-agents**
   - Root agent must import sub-agents to use them:
   ```python
   from .sub_agents.funny_nerd.agent import funny_nerd
   ```

4. **Command Location**
   - Always run `adk web` from the parent directory (`6-multi-agent`), not from inside any agent directory

This structure ensures that ADK can discover and correctly load all agents in the hierarchy.

## Multi-Agent Architecture Options

ADK offers two primary approaches to building multi-agent systems:

### 1. Sub-Agent Delegation Model

Using the `sub_agents` parameter, the root agent can fully delegate tasks to specialized agents:

```python
root_agent = Agent(
    name="manager",
    model="gemini-2.0-flash",
    description="Manager agent",
    instruction="You are a manager agent that delegates tasks to specialized agents...",
    sub_agents=[stock_analyst],
)
```

**Characteristics:**
- Complete delegation - sub-agent takes over the entire response
- The sub-agent decision is final and takes control of the conversation
- Root agent acts as a "router" determining which specialist should handle the query

### 2. Agent-as-a-Tool Model

Using the `AgentTool` wrapper, agents can be used as tools by other agents:

```python
from google.adk.tools.agent_tool import AgentTool

root_agent = Agent(
    name="manager",
    model="gemini-2.0-flash",
    description="Manager agent",
    instruction="You are a manager agent that uses specialized agents as tools...",
    tools=[
        AgentTool(news_analyst),
        get_current_time,
    ],
)
```

**Characteristics:**
- Sub-agent returns results to the root agent
- Root agent maintains control and can incorporate the sub-agent's response into its own
- Multiple tool calls can be made to different agent tools in a single response
- Gives the root agent more flexibility in how it uses the results
