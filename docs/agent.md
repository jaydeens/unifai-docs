---
sidebar_position: 7
title: Creating Agents and Clients
---

# Agent Documentation

## Introduction

We also provide developers with a powerful and flexible framework for building intelligent agents that can interact with users across multiple communication channels. This document provides comprehensive guidance on how to utilize the Agent functionality to create custom AI assistants tailored to your specific needs.

## Table of Contents

1. [Getting Started](#getting-started)
2. [Agent Overview](#agent-overview)
3. [Agent Configuration](#agent-configuration)
   - [Initialization](#initialization)
   - [Prompts Management](#prompts-management)
   - [Model Configuration](#model-configuration)
4. [Client Implementation](#client-implementation)
   - [Available Clients](#available-clients)
   - [Creating Custom Clients](#creating-custom-clients)

## Getting Started

### Installation

```bash
pip install unifai-sdk
```

### Basic Usage

```python
from unifai.agent import Agent
from unifai.client import TelegramClient
 
agent = Agent(api_key="YOUR_UNIFAI_AGENT_API_KEY")

telegram_client = TelegramClient(bot_token="YOUR_TELEGRAM_BOT_TOKEN")
agent.add_client(telegram_client)

agent.run()
```

## Agent Overview

The `Agent` class is the central component of the UnifAI SDK. It coordinates:

- Communication with users through various clients (Telegram, Twitter, Discord, etc.)
- Natural language processing using AI models
- Tools execution for performing actions

Agents are designed to be extensible, allowing developers to customize their behavior through prompts, model selection, etc.

## Agent Configuration

### Initialization

The `Agent` class accepts several parameters during initialization to customize its behavior:

```python
from unifai.agent import Agent

agent = Agent(
    api_key="YOUR_UNIFAI_API_KEY",
    agent_id="",  # Optional: Unique identifier for the agent, useful when running multiple agents that share the same database
    clients=[],  # Optional: List of client instances
    tool_call_concurrency=10,  # Optional: Number of concurrent tool calls
)
```

### Prompts Management

```python
# Get all prompts
all_prompts = agent.get_all_prompts()

# Get a specific prompt
system_prompt = agent.get_prompt("system")

# Set a custom prompt
agent.set_prompt("system", "You are a helpful AI assistant specialized in financial advice.")
```

### Model Configuration

You can specify which AI models to use for different prompts:

```python
# Get the model for a specific prompt
model = agent.get_model("default")  # Returns the model for the default prompt

# Set the default model
agent.set_model("default", "anthropic/claude-3-7-sonnet-20250219")
```

## Client Implementation

The UnifAI SDK supports multiple communication channels through its client architecture. Clients handle the specifics of each platform while presenting a consistent interface to the agent.

### Available Clients

The SDK includes several built-in clients:

- **TelegramClient**: For interacting with users via Telegram
- **TwitterClient**: For interacting with users via Twitter
- **DiscordClient**: For interacting with users via Discord

### Creating Custom Clients

You can create your own clients for platforms not natively supported by implementing the `BaseClient` interface:

```python
from unifai.client.base import BaseClient, MessageContext, Message

class SlackClient(BaseClient):
    """Client implementation for Slack."""
    
    @property
    def client_id(self) -> str:
        """Get client ID"""

    async def start(self):
        """Start the client"""

    async def stop(self):
        """Stop the client"""

    async def receive_message(self) -> Optional[MessageContext]:
        """Receive a message from the client"""

    async def send_message(self, ctx: MessageContext, reply_messages: List[Message]):
        """Send a message"""
```


