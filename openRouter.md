```json
{
  "name": "flexibleChatTool",
  "description": "A flexible tool for multi-model chat generation in Obsidian",
  "parameters": {
    "type": "object",
    "properties": {
      "prompt": {
        "type": "string",
        "description": "The input prompt for the chat generation"
      },
      "model": {
        "type": "string",
        "description": "The model to use (e.g., 'sonnet', 'gpt4'). If not specified, the default model will be used.",
        "default": "sonnet"
      },
      "maxTokens": {
        "type": "integer",
        "description": "Maximum number of tokens to generate",
        "default": 1000
      },
      "temperature": {
        "type": "number",
        "description": "Sampling temperature to use",
        "default": 0.7
      }
    },
    "required": ["prompt"]
  }
}
```

```js
const OPENROUTER_API_URL = 'https://openrouter.ai/api/v1/chat/completions';

const MODEL_PATHS = {
  sonnet: "anthropic/sonnet-3.5",
  gpt4: "openai/gpt-4",
  gemini: "google/gemini-pro",
  claude: "anthropic/haiku-3",
  llama: "meta-llama/llama-3.1-405b-instruct",
  mixtral: "mistralai/mixtral-8x7b-instruct"
  // Add more models as needed
};

async function flexibleChatTool(params, app) {
  const { prompt, model = 'sonnet', maxTokens = 1000, temperature = 0.7 } = params;
  
  const apiKey = app.plugins.getPlugin("obsistants").settings.apiKeys.openRouter;
  const modelPath = MODEL_PATHS[model] || MODEL_PATHS.sonnet;

  try {
    const response = await fetch(OPENROUTER_API_URL, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${apiKey}`,
        'HTTP-Referer': 'https://www.synapticlabs.ai',
        'X-Title': 'Obsistant Tools',
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
    console.error("Error in flexibleChatTool:", error);
    throw new Error(`Failed to generate chat response: ${error.message}`);
  }
}

module.exports = {
  execute: flexibleChatTool
};
```