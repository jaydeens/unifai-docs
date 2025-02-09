---
sidebar_position: 4
title: Using Tools in MCP Clients
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Using Dynamic Tools in MCP Clients

UnifAI SDK provides an [MCP (Model Context Protocol)](https://modelcontextprotocol.io/) server that can be used with MCP-compatible clients like Claude Desktop.

## Setting Up the MCP Server

<Tabs>
  <TabItem value="js" label="JavaScript/TypeScript">

```json
{
  "mcpServers": {
    "unifai-tools": {
      "command": "npx",
      "args": [
        "-y",
        "-p",
        "unifai-sdk",
        "unifai-tools-mcp"
      ],
      "env": {
        "UNIFAI_AGENT_API_KEY": "YOUR_AGENT_API_KEY"
      }
    }
  }
}
```

  </TabItem>
  <TabItem value="py" label="Python">

```json
{
  "mcpServers": {
    "unifai-tools": {
      "command": "uvx",
      "args": [
        "--from",
        "unifai-sdk",
        "unifai-tools-mcp"
      ],
      "env": {
        "UNIFAI_AGENT_API_KEY": "YOUR_AGENT_API_KEY"
      }
    }
  }
}
```

  </TabItem>
</Tabs>

## Configuration Steps

1. Install `npm` (for JavaScript/TypeScript) or `uvx` (for Python) following the official installation instructions.
2. Update your MCP client configuration (for example, in Claude Desktop) with the JSON configuration above.
3. Make sure you put your agent API key in the configuration file.

Once configured, your MCP client will detect and utilize the tools exposed by your toolkit.
