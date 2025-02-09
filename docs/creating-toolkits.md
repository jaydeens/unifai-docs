---
sidebar_position: 5
title: Creating and Serving Toolkits
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Creating Toolkits

Toolkits allow you to make tools available to all UnifAI agents as a service. This section shows you how to create, configure, and run your toolkits using both the JavaScript/TypeScript and Python SDKs.

## Overview

A toolkit is a collection of tools that you register and serve to agents dynamically. It requires a **Toolkit API Key** — which you can get for free from [UnifAI](https://app.unifai.network/).

## Toolkit Creation and Configuration

Let's break down the process of creating and configuring a toolkit:

### Initialize the Toolkit

<Tabs>
<TabItem value="js" label="JavaScript/TypeScript">

```typescript
import { Toolkit } from 'unifai-sdk';

const toolkit = new Toolkit({ apiKey: 'YOUR_TOOLKIT_API_KEY' });
```

</TabItem>
<TabItem value="py" label="Python">

```python
import unifai

toolkit = unifai.Toolkit(api_key='YOUR_TOOLKIT_API_KEY')
```

</TabItem>
</Tabs>

### Update Toolkit Details

You can optionally update the toolkit's name and description:

<Tabs>
<TabItem value="js" label="JavaScript/TypeScript">

```typescript
await toolkit.updateToolkit({ 
    name: "Echo Slam", 
    description: "What's in, what's out." 
});
```

</TabItem>
<TabItem value="py" label="Python">

```python
await toolkit.update_toolkit(
    name="Echo Slam",
    description="What's in, what's out."
)
```

</TabItem>
</Tabs>

### Register Action Handlers

Register actions that your toolkit will provide. The `payloadDescription` can be any string or dictionary that contains enough information for agents to understand the payload format. It acts like API documentation that agents can read to determine what parameters to use.

<Tabs>
<TabItem value="js" label="JavaScript/TypeScript">

```typescript
toolkit.action(
    {
        action: "echo",
        actionDescription: "Echo the message",
        payloadDescription: {
            content: {
              type: "string",
              description: "The message to echo"
            }
        }
    },
    async (ctx, payload) => {
        return ctx.result(`You are agent <${ctx.agentId}>, you said "${payload?.content}".`);
    }
);
```

</TabItem>
<TabItem value="py" label="Python">

```python
@toolkit.action(
    action="echo",
    action_description="Echo the message",
    payload_description={
        "content": {
            "type": "string",
            "description": "The message to echo"
        }
    },
)
def echo(ctx: unifai.ActionContext, payload={}):
    return ctx.Result(f'You are agent <{ctx.agent_id}>, you said "{payload.get("content")}".')
```

</TabItem>
</Tabs>

### Start the Toolkit

Finally, start serving your toolkit:

<Tabs>
<TabItem value="js" label="JavaScript/TypeScript">

```typescript
await toolkit.run();
```

</TabItem>
<TabItem value="py" label="Python">

```python
asyncio.run(toolkit.run())
```

</TabItem>
</Tabs>

Now your toolkit can be discovered and used by all agents connected to UnifAI.

### Complete Example

Here's the complete code putting it all together:

<Tabs>
  <TabItem value="js" label="JavaScript/TypeScript">

```javascript
import { Toolkit } from 'unifai-sdk';

async function main() {
  const toolkit = new Toolkit({ apiKey: 'YOUR_TOOLKIT_API_KEY' });

  // Optionally update the toolkit details
  await toolkit.updateToolkit({ 
    name: "Echo Slam", 
    description: "What's in, what's out." 
  });

  // Register an action handler
  toolkit.action(
    {
      action: "echo",
      actionDescription: "Echo the message",
      payloadDescription: {
        content: { type: "string" }
      }
    },
    async (ctx, payload) => {
      return ctx.result(`You are agent <${ctx.agentId}>, you said "${payload?.content}".`);
    }
  );

  // Start serving the toolkit
  await toolkit.run();
}

main().catch(console.error);
```

  </TabItem>
  <TabItem value="py" label="Python">

```python
import unifai
import asyncio

toolkit = unifai.Toolkit(api_key='YOUR_TOOLKIT_API_KEY')

# Update the toolkit details
asyncio.run(toolkit.update_toolkit(name="Echo Slam", description="What's in, what's out."))

# Register an action handler
@toolkit.action(
    action="echo",
    action_description="Echo the message",
    payload_description={"content": {"type": "string"}},
)
def echo(ctx: unifai.ActionContext, payload={}):
    return ctx.Result(f'You are agent <{ctx.agent_id}>, you said "{payload.get("content")}".')

# Start serving the toolkit
asyncio.run(toolkit.run())
```

  </TabItem>
</Tabs>

## Toolkit Examples

We open source our official toolkits at [unifai-network/unifai-toolkits](https://github.com/unifai-network/unifai-toolkits).
