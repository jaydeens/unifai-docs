---
sidebar_position: 3
title: Using Tools in Your Agents
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Using Dynamic Tools in Your Agents

UnifAI tools enable your agents to find and use tools dynamically. Below are code examples for initializing and using the tools. You will need an **Agent API Key** to use the tools — which you can get for free from [UnifAI](https://app.unifai.network/).

## Initializing Tools

<Tabs>
  <TabItem value="js" label="JavaScript/TypeScript">

```javascript
import { Tools } from 'unifai-sdk';

const tools = new Tools({ apiKey: 'YOUR_AGENT_API_KEY' });
```

  </TabItem>
  <TabItem value="py" label="Python">

```python
import unifai

tools = unifai.Tools(api_key='YOUR_AGENT_API_KEY')
```

  </TabItem>
</Tabs>

## Integrating with LLM

You can pass the initialized tools to any OpenAI-compatible API.
For LLM that are not OpenAI compatible, you can use libraries and services such as [OpenRouter](https://openrouter.ai/docs) that gives you access to most LLMs through a single OpenAI compatible API.

<Tabs>
  <TabItem value="js" label="JavaScript/TypeScript">

```javascript
const messages = [{ content: "What is trending on Google today?", role: "user" }];
const response = await openai.chat.completions.create({
  model: "gpt-4o",
  messages,
  tools: tools.getTools(),
});
messages.push(response.choices[0].message);
if (response.choices[0].message.tool_calls) {
  const results = await tools.callTools(response.choices[0].message.tool_calls);
  messages.push(...results);
  // messages can be passed to the LLM again now
}
```

  </TabItem>
  <TabItem value="py" label="Python">

```python
messages = [{"content": "What is trending on Google today?", "role": "user"}]
response = client.chat.completions.create(
    model="gpt-4o",
    messages=messages,
    tools=tools.get_tools(),
)
messages.append(response.choices[0].message)
if response.choices[0].message.get("tool_calls"):
    results = await tools.call_tools(response.choices[0].message["tool_calls"])
    messages.extend(results)
    # messages can be passed to the LLM again now
```

  </TabItem>
</Tabs>

Passing the tool calls results back to the LLM might get you more function calls, and you can keep calling the tools until you get a response that doesn't contain any tool calls. For example:

<Tabs>
  <TabItem value="js" label="JavaScript/TypeScript">

```javascript
const messages = [{ content: "What is trending on Google today?", role: "user" }];
while (true) {
  const response = await openai.chat.completions.create({
    model: "gpt-4o",
    messages,
    tools: tools.getTools(),
  });
  messages.push(response.choices[0].message);
  const results = await tools.callTools(response.choices[0].message.tool_calls);
  if (results.length === 0) break;
  messages.push(...results);
}
```

  </TabItem>
  <TabItem value="py" label="Python">

```python
messages = [{"content": "What is trending on Google today?", "role": "user"}]
while True:
    response = client.chat.completions.create(
        model="gpt-4o",
        messages=messages,
        tools=tools.get_tools(),
    )
    messages.append(response.choices[0].message)
    results = await tools.call_tools(response.choices[0].message.tool_calls)
    if len(results) == 0:
        break
    messages.extend(results)
```

  </TabItem>
</Tabs>

## System Prompt

Most models are good at searching and using UnifAI tools without any special instructions.
But we find using a simple system prompt helps the model to search and use the tools more effectively.

An example system prompt:

```
You are a personal assistant capable of doing many things with your tools. 
When you are given a task you cannot finish right now (like something you don't know, 
or requires you to take some action), try find appropriate tools to do it.

When searching for tools, try to think what tools might be useful and use relevant 
generic keywords rather than putting all the details/numbers into the query, because 
you are finding the tool, not solving the problem itself. If you failed to find 
the appropriate tools, you can try changing query and search again.
```
