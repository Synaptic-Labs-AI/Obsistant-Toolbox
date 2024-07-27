# llmBlock: LLM Integration for Obsidian Tools

## Overview

llmBlock is a component for integrating LLM capabilities into Obsidian tools. This note contains all necessary elements to use llmBlock in your tools.

## JSON Schema

```json
{
  "name": "llmBlock",
  "description": "LLM component for Obsidian tools",
  "parameters": {
    "type": "object",
    "properties": {
      "prompt": {
        "type": "string",
        "description": "Input prompt for the LLM"
      },
      "model": {
        "type": "string",
        "description": "Model to use (e.g., '#sonnet', '#gpt4')",
        "default": "#sonnet"
      },
      "maxTokens": {
        "type": "integer",
        "description": "Maximum tokens to generate",
        "default": 1000
      },
      "temperature": {
        "type": "number",
        "description": "Sampling temperature",
        "default": 0.7
      }
    },
    "required": ["prompt"]
  }
}
```

## JavaScript Implementation

```javascript
const OPENROUTER_API_URL = 'https://openrouter.ai/api/v1/chat/completions';

const MODEL_PATHS = {
  "#mini": "openai/gpt-4o-mini",
  "#gpt": "openai/gpt-4o",
  "#haiku": "anthropic/haiku-3",
  "#sonnet": "anthropic/sonnet-3.5",
  "#mythomax": "mythomax-13b",
  "#flash": "google/gemini-flash-1.5",
  "#gemini": "google/google/gemini-pro-1.5",
  "#perplexity": "perplexity/llama-3-sonar-large-32k-online",
  "#mistral": "mistralai/mistral-large",
  "#llama": "meta-llama/llama-3.1-405b-instruct",
  "#nemo": "mistralai/mistral-nemo",
  "#nemotron": "nvidia/nemotron-4-340b-instruct"
};

async function llmBlock(params, app) {
  const { prompt, model = '#sonnet', maxTokens = 1000, temperature = 0.7 } = params;
  
  const apiKey = app.plugins.getPlugin("obsistants").settings.apiKeys.openRouter;
  const modelPath = MODEL_PATHS[model] || MODEL_PATHS['#sonnet'];

  try {
    const response = await fetch(OPENROUTER_API_URL, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${apiKey}`,
        'HTTP-Referer': 'https://obsidian.md',
        'X-Title': 'Obsidian Obsistants Plugin',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        model: modelPath,
        messages: [{ role: 'user', content: prompt }],
        max_tokens: maxTokens,
        temperature: temperature
      })
    });

    if (!response.ok) {
      throw new Error(`API call failed: ${response.statusText}`);
    }

    const data = await response.json();
    return data.choices[0].message.content.trim();
  } catch (error) {
    console.error("Error in llmBlock:", error);
    throw new Error(`Failed to generate LLM response: ${error.message}`);
  }
}
```

## Integration Instructions

To use llmBlock in your Obsidian tool:

1. Copy the JSON schema and JavaScript implementation into your tool's markdown note.
2. In your tool's main function, use llmBlock as follows:

```javascript
async function yourToolFunction(params, app) {
  try {
    const llmResponse = await llmBlock({
      prompt: "Your custom prompt here",
      model: "#sonnet", // or any other model tag
      maxTokens: 1000,  // optional
      temperature: 0.7  // optional
    }, app);

    // Process the llmResponse as needed for your tool
    return `Your tool's output: ${llmResponse}`;
  } catch (error) {
    console.error("Error in yourToolFunction:", error);
    return `Error: ${error.message}`;
  }
}
```

3. Ensure your tool's JSON schema includes necessary parameters for llmBlock usage, as well as any additional required for the specific tool.

## Usage Example

```json
{
  "name": "summarizeTool",
  "description": "Summarize text using various LLM models",
  "parameters": {
    "type": "object",
    "properties": {
      "text": {
        "type": "string",
        "description": "Text to summarize"
      },
      "model": {
        "type": "string",
        "description": "LLM model to use (e.g., '#sonnet', '#gpt')",
        "default": "#sonnet"
      },
      "maxTokens": {
        "type": "string",
        "description": "Maximum tokens for the summary (e.g., '#1000')",
        "default": "#1000"
      }
    },
    "required": ["text"]
  }
}
```

```javascript
async function summarizeTool(params, app) {
  const { text, model = "#sonnet", maxTokens = "#1000" } = params;
  
  const MODEL_PATHS = {
    "#mini": "openai/gpt-4o-mini",
    "#gpt": "openai/gpt-4o",
    "#haiku": "anthropic/haiku-3",
    "#sonnet": "anthropic/sonnet-3.5",
    "#mythomax": "mythomax-13b",
    "#flash": "google/gemini-flash-1.5",
    "#gemini": "google/google/gemini-pro-1.5",
    "#perplexity": "perplexity/llama-3-sonar-large-32k-online",
    "#mistral": "mistralai/mistral-large",
    "#llama": "meta-llama/llama-3.1-405b-instruct",
    "#nemo": "mistralai/mistral-nemo",
    "#nemotron": "nvidia/nemotron-4-340b-instruct"
  };

  // Extract numeric value from maxTokens
  const tokenLimit = parseInt(maxTokens.replace('#', ''));

  try {
    const summary = await llmBlock({
      prompt: `Summarize the following text concisely:\n\n${text}`,
      model: model,
      maxTokens: tokenLimit
    }, app);
    return `Summary ${model}:\n\n${summary}`;
  } catch (error) {
    console.error("Error in summarizeTool:", error);
    return `Error: ${error.message}`;
  }
}

async function llmBlock(params, app) {
  const { prompt, model = '#sonnet', maxTokens = 1000, temperature = 0.7 } = params;
  
  const apiKey = app.plugins.getPlugin("obsistants").settings.apiKeys.openRouter;
  const modelPath = MODEL_PATHS[model] || MODEL_PATHS['#sonnet'];

  try {
    const response = await fetch("https://openrouter.ai/api/v1/chat/completions", {
      method: "POST",
      headers: {
        "Authorization": `Bearer ${apiKey}`,
        "HTTP-Referer": "https://www.synapticlabs.ai",
        "X-Title": "Obsistant Tool",
        "Content-Type": "application/json"
      },
      body: JSON.stringify({
        model: modelPath,
        messages: [{ role: "user", content: prompt }],
        max_tokens: maxTokens,
        temperature: temperature
      })
    });

    if (!response.ok) {
      throw new Error(`API call failed: ${response.statusText}`);
    }

    const data = await response.json();
    return data.choices[0].message.content.trim();
  } catch (error) {
    console.error("Error in llmBlock:", error);
    throw new Error(`Failed to generate LLM response: ${error.message}`);
  }
}

module.exports = {
  execute: summarizeTool
};
```
