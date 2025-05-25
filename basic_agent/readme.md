## What is an ADK Agent?

The `LlmAgent` (often aliased simply as `Agent`) is a core component in ADK that acts as the "thinking" part of your application. It leverages the power of a Large Language Model (LLM) for:
- Reasoning
- Understanding natural language
- Making decisions
- Generating responses
- Interacting with tools

Unlike deterministic workflow agents that follow predefined paths, an `LlmAgent`'s behavior is non-deterministic. It uses the LLM to interpret instructions and context, deciding dynamically how to proceed, which tools to use (if any), or whether to transfer control to another agent.

## Basic Agent Structure
The basic structure of an ADK agent is straightforward. Here’s a simple example of a basic agent:

```
parent_folder/
    agent_folder/         # This is your agent's package directory
        __init__.py       # Must import agent.py
        agent.py          # Must define root_agent
        .env              # Environment variables
```