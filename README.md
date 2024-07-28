# Obsistant-Toolbox

Obsistant Tools are powerful extensions for the Obsistants plugin. These tools allow you to create custom functionality that integrates seamlessly with Obsidian to execute AI-enhanced code to perform a specific, or complex task - enhancing your note-taking experience and productivity.

At their core, Obsistant Tools are a combination of two key components:
1. A JSON schema that defines the tool's structure and parameters
2. A JavaScript function that implements the tool's functionality

What sets Obsistant Tools apart is their ability to interact directly with your Obsidian vault, accessing and manipulating your notes, and connect to anything with an API. This deep integration allows for the creation of tools that feel like native Obsidian features, tailored to your specific workflow.

## Why Use Obsistant Tools?

Obsidian is already a powerful tool for knowledge management, but everyone's workflow is unique. Obsistant Tools allow you to extend and customize Obsidian to fit your exact needs. Here are some compelling reasons to use and develop Obsistant Tools:

1. **Personalization**: Create tools that cater to your specific note-taking style and research methods.

2. **Automation**: Streamline repetitive tasks and save time by automating common operations.

3. **Integration**: Connect Obsidian with external services and APIs to bring all your information into one place.

4. **Enhanced Productivity**: Develop tools that fill gaps in your workflow, making you more efficient and productive.

5. **Community Contribution**: Share your tools with the Obsidian community, helping others and getting feedback to improve your coding skills.

6. **Learning Opportunity**: Developing Obsistant Tools is an excellent way to learn or improve your JavaScript skills in a practical, applied setting.

Whether you're a seasoned developer looking to optimize your note-taking workflow, or a curious Obsidian user eager to dip your toes into programming, Obsistant Tools offer a world of possibilities. In the following sections, we'll guide you through the process of creating your own Obsistant Tools, from simple scripts to complex integrations, empowering you to take control of your knowledge management system.

---

# 2. Basic Structure of an Obsistant Tool

Every Obsistant Tool consists of two main parts: a JSON Schema and a JavaScript Function. Don't worry if you're not familiar with these terms - we'll break them down step by step!

## JSON Schema

Think of the JSON Schema as a recipe card for your tool. It tells your Obsistant what your tool is called, what it does, and what ingredients (parameters) it needs to work.

### Structure of a JSON Schema

```json
{
 "name": "yourToolName",
 "description": "A brief description of what your tool does",
 "parameters": {
   "type": "object",
   "properties": {
     "paramName1": {
       "type": "string",
       "description": "Description of the first parameter"
     },
     "paramName2": {
       "type": "integer",
       "description": "Description of the second parameter"
     }
   },
   "required": ["paramName1"]
 }
}
```

**Let's break this down:**

1. `"name"`: This is the name of your tool. Choose something descriptive!
2. `"description"`: Explain what your tool does in a sentence or two.
3. `"parameters"`: This section describes any inputs your tool needs.
4. `"type"`: "object": This tells Obsidian that your parameters are grouped together.
5. `"properties"`: Here, you list each parameter your tool uses.

For each parameter, you specify its "type" (like "string" for text, "integer" for whole numbers) and provide a "description".

6. `"required"`: List any parameters that are absolutely necessary for your tool to work.

### Example: Daily Journal Template
Let's create a JSON Schema for a tool that generates a daily journal template:
```json
"name": "createDailyJournal",
  "description": "Creates a new note with a daily journal template",
  "parameters": {
    "type": "object",
    "properties": {
      "date": {
        "type": "string",
        "description": "The date for the journal entry (YYYY-MM-DD)"
      },
      "mood": {
        "type": "string",
        "description": "Your mood for the day"
      }
    },
    "required": ["date"]
  }
}
```
  
In this example, our tool needs a date (which is required) and can optionally include a mood.

## JavaScript Function
The JavaScript Function is where the magic happens. This is the actual code that performs the task you want.

**Structure of a JavaScript Function**
```js
async function yourToolName(params) {
  // Your code goes here
  // Use params.paramName1, params.paramName2, etc. to access parameters
  
  // Always return a string at the end
  return "Result of your tool";
}
```

### Key Points

- The function name should match the "name" in your JSON Schema.
- Use async function because many operations in Obsidian are asynchronous.
- The params object contains all the parameters you defined in your JSON Schema.
- Your function should always return a string.

**Example: Daily Journal Template Function**
Let's write a simple function for our daily journal template:

```js
async function createDailyJournal(params) {
  // Get the date and mood from params
  const date = params.date;
  const mood = params.mood || "Not specified";

  // Create the journal template
  const template = `# Journal Entry for ${date}

Mood: ${mood}

## What I'm grateful for today:
1. 
2. 
3. 

## Today's main goals:
- [ ] 
- [ ] 
- [ ]

## Reflections:

`;

  // Create a new note with the template
  const fileName = `Journal_${date}.md`;
  await app.vault.create(fileName, template);

  return `Created journal entry for ${date}`;
}
```

**This function:**
1. Gets the date and mood from the parameters.
2. Creates a template string for the journal entry.
3. Creates a new note in your Obsidian vault with the template.
4. Returns a confirmation message.

### 2.3 Putting It All Together
To create an Obsistant Tool, you combine the JSON Schema and the JavaScript Function in a single file. 

Here's how it looks:

#### **Daily Journal Creator**

```json
{
  "name": "createDailyJournal",
  "description": "Creates a new note with a daily journal template",
  "parameters": {
    "type": "object",
    "properties": {
      "date": {
        "type": "string",
        "description": "The date for the journal entry (YYYY-MM-DD)"
      },
      "mood": {
        "type": "string",
        "description": "Your mood for the day"
      }
    },
    "required": ["date"]
  }
}
javascriptCopyasync function createDailyJournal(params) {
  const date = params.date;
  const mood = params.mood || "Not specified";

  const template = `# Journal Entry for ${date}

Mood: ${mood}

## What I'm grateful for today:
1. 
2. 
3. 

## Today's main goals:
- [ ] 
- [ ] 
- [ ]

## Reflections:

`;

  const fileName = `Journal_${date}.md`;
  await app.vault.create(fileName, template);

  return `Created journal entry for ${date}`;
}
```

And that's it! You've created your first Obsistant Tool. The JSON Schema defines what your tool needs, and the JavaScript Function makes it work.

Remember, this is just a basic example. As you get more comfortable, you can create more complex tools that do all sorts of amazing things in Obsidian. Happy coding!

# Snippets

Snippets in Obsistant Tools are pre-written, reusable pieces of code that perform specific functions or encapsulate common operations. Think of them as your coding lego blocks – always ready to help you quickly build what you want without having to recode the same thing multiple times.

### Definition:
A snippet is a self-contained unit of code that can be easily integrated into larger Obsistant Tools. It can be either a function that performs a specific task or a class that encapsulates related functionality and data.

### Purpose:
Snippets serve three main purposes in Obsistant Tools development:

1. **Code Reuse**: Instead of writing the same code over and over, you can use snippets to quickly implement common functionalities. This is like having a set of trusted recipes that you can use whenever you need them.

2. **Standardization**: Snippets ensure that common operations are performed consistently across different tools. This standardization helps maintain code quality and reduces errors.

3. **Simplifying Complex Operations**: Some operations, like interacting with APIs or performing complex data manipulations, can be intricate. Snippets encapsulate this complexity, providing a simple interface for tool developers.

## Types of Snippets

Obsistant Tools, we have two main types of snippets: Function Snippets and Class Snippets. Let's break these down and see how they differ.

### Function Snippets

Okay, so what's a function snippet? 

A function snippet is a single, self-contained function that performs a specific task. It's like a specialized tool in your toolbox - you grab it when you need to do one particular job.

Structure:
```javascript
function doSomething(params) {
    // Do the thing
    return result;
}
```

When to use:
- When you have a single, well-defined task
- When you don't need to maintain state (memory) between operations
- For simple, straightforward operations

**Example** 
Let's look at our `llmBlock` function snippet:

```javascript
async function llmBlock(params, app) {
  const { prompt, model = '#sonnet', maxTokens = 1000, temperature = 0.7 } = params;
  
  // API call logic here...

  return generatedText;
}
```

This function does one thing: it takes some parameters, makes an API call, and returns the generated text. It doesn't need to remember anything between calls, and it doesn't have any internal state.

### Class Snippets

Now, what about class snippets?

A class snippet is a blueprint for creating objects that can have multiple related methods and maintain internal state. It's like a multi-tool that can do several related tasks and remember things between uses.

Structure:
```javascript
class SomeTool {
    constructor(initialState) {
        this.state = initialState;
    }

    doThing1() {
        // Do thing 1
    }

    doThing2() {
        // Do thing 2
    }
}
```

When to use:
- When you have multiple related operations
- When you need to maintain state between operations
- For more complex, stateful operations

**Example**
Let's create a hypothetical `NoteManager` class snippet:

```javascript
class NoteManager {
    constructor(app) {
        this.app = app;
        this.currentNote = null;
    }

    async getCurrentNote() {
        // Logic to get current note
        this.currentNote = /* ... */;
        return this.currentNote;
    }

    async updateNote(content) {
        if (!this.currentNote) await this.getCurrentNote();
        // Logic to update the note
    }

    async createNote(name, content) {
        // Logic to create a new note
    }
}
```

This class maintains state (the `currentNote`) and has multiple related methods for managing notes. It's more complex than a single function, but it can handle a variety of note-related tasks.

### Comparing Function and Class Snippets

Now, let's compare these two types:

1. **Complexity**: 
   - Function snippets are simpler, great for straightforward tasks.
   - Class snippets can handle more complex, multi-step operations.

2. **State Management**:
   - Function snippets don't maintain state between calls.
   - Class snippets can maintain state in their properties.

3. **Usage**:
   - Function snippets are used directly: `result = functionSnippet(params);`
   - Class snippets need to be instantiated: `const manager = new ClassSnippet(); manager.doSomething();`

4. **Organizatio**n:
   - Function snippets are good for organizing unrelated utilities.
   - Class snippets are great for grouping related functionality.

5. **Flexibility**:
   - Function snippets are more flexible and can be easily composed.
   - Class snippets provide a more structured approach to complex tasks.

In practice, you'll likely use both types in your Obsistant Tools. Function snippets like `llmBlock` are great for discrete operations like API calls, while class snippets like our hypothetical `NoteManager` are perfect for managing more complex, stateful operations.

Remember, the goal is to choose the right tool for the job. As you develop more Obsistant Tools, you'll get a feel for when to use each type of snippet. Don't be afraid to experiment!

## Anatomy of a Snippet

The `llmBlock` snippet is a function snippet that allows Obsistant Tools to interact with various language models through the OpenRouter API. Let's break it down piece by piece.

### JSON Schema Breakdown

First, let's look at the JSON Schema:

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

Let's break this down:

1. `name`: This is the identifier for the snippet. It's how other parts of your tool will refer to this snippet.

2. `description`: A brief explanation of what the snippet does.

3. `parameters`: This object defines the inputs that the snippet expects.
   - `prompt`: The main input text for the language model. It's the only required parameter.
   - `model`: Allows selection of different language models. It has a default value of "#sonnet".
   - `maxTokens`: Controls the length of the generated text. Default is 1000.
   - `temperature`: Affects the randomness of the output. Default is 0.7.

This schema provides a clear contract for how to use the `llmBlock` snippet, making it easy for developers to understand what inputs it expects and what options are available.

### JavaScript Implementation

Now, let's look at the JavaScript implementation:

```javascript
const OPENROUTER_API_URL = 'https://openrouter.ai/api/v1/chat/completions';

const MODEL_PATHS = {
  "#mini": "openai/gpt-4o-mini",
  "#gpt": "openai/gpt-4o",
  "#haiku": "anthropic/haiku-3",
  "#sonnet": "anthropic/sonnet-3.5",
  // ... other models ...
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

Key points in this implementation:

1. **It's patient**: The function is what we call "asynchronous". This is like being able to start a task and then do other things while waiting for it to finish. Imagine starting the dishwasher and then going to watch TV instead of standing there waiting for it to finish.

2. **It unpacks neatly**: When you give the function information (we call these "parameters"), it neatly unpacks them. It's like getting a package and taking out exactly what you need, leaving the rest in the box. If something's missing, it uses a default item instead.

3. **It keeps secrets safe**: The function needs a special key (API key) to talk to the language service. Instead of asking you for this key every time, it looks for it in a safe place where the Obsistants plugin keeps it. It's like having a spare house key hidden somewhere only you know about.

4. **It speaks many languages**: We have a list (MODEL_PATHS) that matches simple names like "#sonnet" to the complex names the API understands. It's like having a codebook that translates "chocolate" to "item #2546" for a vending machine.

5. **It makes a phone call**: To get the generation, the function uses something called `fetch`. This is like making a phone call to the language model and asking for help.

6. **It handles problems gracefully**: If something goes wrong (like if the language model is busy), the function has a way to deal with it without crashing.

7. **It gives you the answer**: After all this, the function returns the text. It's like finally getting your order at a restaurant - here's the meal you asked for!


### How llmBlock Integrates

The `llmBlock` snippet showcases several key features of well-designed snippets:

1. **Abstraction**: It hides the complexity of API interaction behind a simple interface.
2. **Flexibility**: Users can easily switch between different language models.
3. **Error Handling**: It provides robust error handling and logging.
4. **Default Values**: It uses sensible defaults while allowing customization.

Remember, a good snippet should simplify complex operations and provide a clear, easy-to-use interface for other developers.

## Using Snippets in Obsistant Tools

When building your Obsistant Tools, you can leverage both function and class snippets to enhance your tool's capabilities. Here's how to use them:

### Accessing Snippets

All snippets are accessed through the Obsistants plugin:

```javascript
app.plugins.getPlugin("obsistants").snippets
```

This is your gateway to all available snippets.

### Using Function Snippets

Function snippets are called directly. Here's how to use the `llmBlock` function snippet:

```javascript
async function myTool(params) {
  try {
    const result = await app.plugins.getPlugin("obsistants").snippets.llmBlock({
      prompt: "Translate to French: Hello, world!",
      model: "#sonnet",
      maxTokens: 50
    });
    return `Translation: ${result}`;
  } catch (error) {
    console.error("Error in myTool:", error);
    return `Error: ${error.message}`;
  }
}
```

Key points:
- Call the function directly through the snippets object.
- Use `await` as most snippets are asynchronous.
- Always wrap snippet calls in a try/catch block for error handling.

### Using Class Snippets

Class snippets need to be instantiated before use. Here's an example with a hypothetical `NoteManager` class snippet:

```javascript
async function myNoteTool(params) {
  const NoteManager = app.plugins.getPlugin("obsistants").snippets.NoteManager;
  const noteManager = new NoteManager(app);

  try {
    const currentNote = await noteManager.getCurrentNote();
    const content = await noteManager.getNoteContent(currentNote);
    return `Current note content: ${content.substring(0, 100)}...`;
  } catch (error) {
    console.error("Error in myNoteTool:", error);
    return `Error: ${error.message}`;
  }
}
```

Key points:
- First, get the class from the snippets object.
- Instantiate the class, usually passing `app` as an argument.
- Use the instance's methods as needed.

### Combining Snippets

You can use multiple snippets in a single tool. Here's an example that combines both types:

```javascript
async function myCombinedTool(params) {
  const NoteManager = app.plugins.getPlugin("obsistants").snippets.NoteManager;
  const noteManager = new NoteManager(app);

  try {
    // Use NoteManager to get current note
    const currentNote = await noteManager.getCurrentNote();
    const content = await noteManager.getNoteContent(currentNote);

    // Use llmBlock to summarize
    const summary = await app.plugins.getPlugin("obsistants").snippets.llmBlock({
      prompt: `Summarize: ${content.substring(0, 500)}...`,
      model: "#sonnet",
      maxTokens: 100
    });

    // Create a new note with the summary
    await noteManager.createNote("Summary", summary);

    return `Summary created successfully.`;
  } catch (error) {
    console.error("Error in myCombinedTool:", error);
    return `Error: ${error.message}`;
  }
}
```

Remember:
1. Always access snippets through `app.plugins.getPlugin("obsistants").snippets`.
2. Use `await` with asynchronous snippet functions.
3. Instantiate class snippets before using their methods.
4. Wrap snippet usage in try/catch blocks for error handling.
5. You can combine multiple snippets to create powerful, complex tools.

# APIs

## Obsidian

1. File Operations:
   - `app.vault.getAbstractFileByPath()`: Retrieve file or folder by path
   - `app.vault.getMarkdownFiles()`: Get all Markdown files in the vault
   - `app.vault.getFiles()`: Get all files in the vault
   - `app.vault.read()`: Read file content
   - `app.vault.cachedRead()`: Read file content from cache
   - `app.vault.modify()`: Modify file content
   - `app.vault.create()`: Create a new file
   - `app.vault.createFolder()`: Create a new folder
   - `app.vault.delete()`: Delete a file or folder
   - `app.vault.trash()`: Move a file or folder to trash
   - `app.vault.rename()`: Rename a file or folder
   - `app.vault.adapter.exists()`: Check if a file or folder exists

2. User Interface:
   - `addRibbonIcon()`: Add an icon to the left ribbon
   - `addStatusBarItem()`: Add an item to the status bar
   - `addCommand()`: Add a command to the command palette
   - `registerView()`: Register a custom view
   - `addSettingTab()`: Add a settings tab for the plugin
   - `Menu class`: Create context menus
   - `Modal class`: Create modal dialogs
   - `Notice class`: Display notifications
   - `setIcon()`: Set an icon for UI elements

3. Events:
   - `app.vault.on('create')`: Listen for file creation events
   - `app.vault.on('modify')`: Listen for file modification events
   - `app.vault.on('delete')`: Listen for file deletion events
   - `app.workspace.on('file-open')`: Listen for file open events
   - `app.workspace.on('layout-change')`: Listen for layout change events
   - `app.workspace.on('active-leaf-change')`: Listen for active leaf change events
   - `registerEvent()`: Register event listeners
   - `registerInterval()`: Register interval-based tasks

4. Workspace Interaction:
   - `app.workspace.getActiveFile()`: Get the currently active file
   - `app.workspace.getActiveViewOfType()`: Get the active view of a specific type
   - `app.workspace.iterateCodeMirrors()`: Iterate through all CodeMirror instances
   - `app.workspace.getLeaf()`: Get a workspace leaf
   - `app.workspace.activeLeaf.openFile()`: Open a file in the active leaf

6. Editor Interaction:
   - `editor.getSelection()`: Get the selected text
   - `editor.replaceSelection()`: Replace the selected text
   - `editor.getCursor()`: Get the current cursor position
   - `editor.setCursor()`: Set the cursor position

7. Data Access:
   - `app.metadataCache.getFileCache()`: Get metadata for a file
   - `app.metadataCache.getCache()`: Get metadata for all files