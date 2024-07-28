# Obsistants-Toolbox

Obsistants Tools are powerful extensions that seamlessly integrate with the Obsidian ecosystem, empowering you to create custom, AI-enhanced functionality. These tools bridge the gap between your unique workflow needs and Obsidian's core features, allowing you to automate tasks, manipulate notes, and connect to external services with unprecedented ease.

## Core Components

At the heart of every Obsistants Tool are two key elements:

1. **JSON Schema**: This defines the tool's structure and parameters, acting as a blueprint for its functionality.
2. **JavaScript Function**: This implements the tool's logic, bringing its capabilities to life.

This dual-component structure allows for a clear separation between the tool's interface (what the AI sees) and its implementation (what it does behind the scenes).

## The Power of Integration

What sets Obsistants Tools apart is their deep integration with your Obsidian vault. They can:

- Access and manipulate your notes directly
- Interact with Obsidian's internal APIs
- Connect to external services and APIs

This level of integration allows you to create tools that feel like native Obsidian features, tailored precisely to your workflow.

## Why Embrace Obsistants Tools?

1. **Personalization**: Craft tools that align perfectly with your unique note-taking style and research methods.
2. **Automation**: Streamline repetitive tasks, saving valuable time and mental energy.
3. **Integration**: Bring external information and services directly into your Obsidian workspace.
4. **Enhanced Productivity**: Fill gaps in your workflow, making you more efficient and effective.
5. **Community Contribution**: Share your creations, helping others while honing your skills.
6. **Learning Opportunity**: Improve your JavaScript skills in a practical, applied setting.

Whether you're a seasoned developer or a curious beginner, Obsistants Tools offer a world of possibilities to enhance your knowledge management system.

## Understanding Tool-Assistant Interaction

To create truly effective Obsistants Tools, it's crucial to understand how they interact with the AI assistant. This interaction is primarily facilitated through the JSON schema and the tool's return value.

### The JSON Schema as Context

The JSON schema serves as the primary interface between the AI assistant and your tool. Key points to remember:

- The assistant can access and interpret the JSON schema.
- The assistant cannot see or access the actual JavaScript code of your tool.
- Any comments or notes within your tool's code are invisible to the assistant.

This means that all context and instructions for the assistant must be provided through the JSON schema or the tool's return value.

### Guiding Assistant Behavior

You can effectively steer the assistant's behavior using both the JSON schema and the tool's return value:

1. **In the JSON Schema:**
   Include instructions for the assistant in the `description` field or as additional properties. For example:

   ```json
   {
     "name": "createNote",
     "description": "Creates a new note. If 'name' is not provided, ask the user for a name before running the tool.",
     "parameters": {
       "type": "object",
       "properties": {
         "name": {
           "type": "string",
           "description": "Name of the new note"
         }
       }
     }
   }
   ```

2. **In the Tool's Return Value:**
   Include both the operation result and additional instructions for the assistant. For instance:

   ```javascript
   return JSON.stringify({
     result: "Note created successfully",
     instruction: "Format this result as a bulleted list and ask the user if they want to open the new note."
   });
   ```

### Crafting Effective Interactions

Think of these schema descriptions and return instructions as mini-prompts for the assistant. They allow you to:

- Provide context-specific guidance
- Handle edge cases gracefully
- Create intuitive and interactive user experiences

By creatively using these interactions, you can significantly enhance your tools' functionality and user-friendliness. For example:

- Use the schema to provide default values or suggest input formats.
- Include error handling instructions in your return values.
- Guide the assistant to ask follow-up questions based on the tool's output.

Remember, the goal is to create a seamless experience where the assistant and your tools work in harmony to meet the user's needs effectively. Don't hesitate to experiment with different approaches – the most innovative solutions often lead to the best user experiences.

As you develop your tools, always consider how the assistant will interpret and use the information you provide. This mindset will help you create tools that not only function well but also integrate smoothly into the AI-assisted workflow, truly enhancing your Obsidian experience.

---

# 2. Basic Structure of an Obsistants Tool

Every Obsistants Tool is stored as a markdown note, consisting of a JSON Schema, a JavaScript Function, and accompanying documentation. This structure allows you to store tools as readable, self-documented markdown files within your Obsidian vault.

The file structure for Tools typically looks like this:
```
Obsistants/
└── Tools/
    ├── createDailyJournal.md
    ├── summarizeNote.md
    └── translateText.md
```
Each tool is a separate markdown file within the Tools directory.

Here's an example of what the contents of a tool file (e.g., `createDailyJournal.md`) might look like:

~~~markdown
# createDailyJournal

This tool creates a new note with a daily journal template.

## JSON Schema

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
```

## JavaScript Function

```javascript
async function createDailyJournal(params) {
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

## Parameters

- `date` (string, required): The date for the journal entry in YYYY-MM-DD format.
- `mood` (string, optional): Your mood for the day. If not provided, it defaults to "Not specified".

## Returns

- A string confirming the creation of the journal entry.

## Description

This tool creates a new markdown note with a daily journal template. The template includes sections for the date, mood, gratitude list, goals, and reflections. The note is created in the root of your Obsidian vault with the filename format `Journal_YYYY-MM-DD.md`.

## Error Handling

If an error occurs during the creation of the note, the error will be thrown and should be handled by the calling function.
~~~

This structure allows you to store tools as readable, self-documented markdown files within your Obsidian vault, just like snippets. The documentation provides clear information about the tool's purpose, parameters, return value, and any error handling considerations.


## JSON Schema


Think of the JSON Schema as a recipe card for your tool. It tells your Obsistants what your tool is called, what it does, and what ingredients (parameters) it needs to work.

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
To create an Obsistants Tool, you combine the JSON Schema and the JavaScript Function in a single file. 

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
```

```javascript
async function createDailyJournal(params) {
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

And that's it! You've created your first Obsistants Tool. The JSON Schema defines what your tool needs, and the JavaScript Function makes it work.

Remember, this is just a basic example. As you get more comfortable, you can create more complex tools that do all sorts of amazing things in Obsidian. Happy coding!

> [!important]
> Tools must contain both a `json` and a `js` (or `javascript`) code block. If you write any of your code *outside of a code block*, it simply will not run!
> 
> Example:
> ~~~
> ```json
> {
>   "name": "myTool",
>   "description": "Description of my tool"
> }
> ```
> 
> ```js
> async function myTool(params) {
>   // Your code here
> }
> ```
> ~~~
# Snippets

Snippets in Obsistants Tools are pre-written, reusable pieces of code that perform specific functions or encapsulate common operations. Think of them as your coding lego blocks – always ready to help you quickly build what you want without having to recode the same thing multiple times.

Snippets are organized in a similar directory structure as tools:
```
Obsistants/
└── Snippets/
    ├── llmBlock.md
    ├── NoteManager.md
    └── DateFormatter.md
```
## Definition
A snippet is a self-contained unit of code that can be easily integrated into larger Obsistants Tools. It can be either a function that performs a specific task or a class that encapsulates related functionality and data.

## Purpose
Snippets serve three main purposes in Obsistants Tools development:

1. **Code Reuse**: Instead of writing the same code over and over, you can use snippets to quickly implement common functionalities. This is like having a set of trusted recipes that you can use whenever you need them.

2. **Standardization**: Snippets ensure that common operations are performed consistently across different tools. This standardization helps maintain code quality and reduces errors.

3. **Simplifying Complex Operations**: Some operations, like interacting with APIs or performing complex data manipulations, can be intricate. Snippets encapsulate this complexity, providing a simple interface for tool developers.


# Snippets in Obsistants Tools



In Obsistants Tools, snippets are stored as markdown notes with a specific structure. Let's break down the anatomy of a snippet using the `llmBlock` function as an example:

~~~markdown
# llmBlock

```javascript
const llmBlock = async ({ prompt, model = '#sonnet', maxTokens = 1000, temperature = 0.7 }) => {
  try {
    // API call logic here...
    const generatedText = await makeAPICall(prompt, model, maxTokens, temperature);
    return generatedText;
  } catch (error) {
    console.error("Error in llmBlock:", error);
    throw new Error(`Failed to generate LLM response: ${error.message}`);
  }
};
```

This snippet allows Obsistants Tools to interact with various language models through the OpenRouter API.

## Parameters

- `prompt` (string): The input text for the language model.
- `model` (string, optional): The model to use (e.g., '#sonnet', '#gpt4'). Defaults to '#sonnet'.
- `maxTokens` (integer, optional): Maximum tokens to generate. Defaults to 1000.
- `temperature` (number, optional): Sampling temperature. Defaults to 0.7.

## Returns

- A string containing the generated text from the language model.

## Error Handling

This snippet uses a try-catch block to handle errors. If an error occurs during the API call, it logs the error and throws a new error with a descriptive message.


Let's break this down:

1. The snippet starts with a markdown heading (`#`) containing the snippet's name.
2. The actual code is enclosed in a markdown code block, specified as JavaScript (` ```javascript `).
3. After the code block, you can include additional information such as parameter descriptions, return value explanations, and notes on error handling.
~~~
This structure allows you to store snippets as readable, self-documented markdown files within your Obsidian vault.


## Types of Snippets

In Obsistants Tools, we have two main types of snippets: Function Snippets and Class Snippets. Let's break these down and see how they differ.

### Function Snippets

A function snippet is a single, self-contained function that performs a specific task. It's like a specialized tool in your toolbox - you grab it when you need to do one particular job.

Structure:
```javascript
const doSomething = async (params) => {
    // Do the thing
    return result;
};
```

When to use:
- When you have a single, well-defined task
- When you don't need to maintain state (memory) between operations
- For simple, straightforward operations

**Example** 
Let's look at our `llmBlock` function snippet:

```javascript
const llmBlock = async ({ prompt, model = '#sonnet', maxTokens = 1000, temperature = 0.7 }) => {
  try {
    // API call logic here...
    const generatedText = await makeAPICall(prompt, model, maxTokens, temperature);
    return generatedText;
  } catch (error) {
    console.error("Error in llmBlock:", error);
    throw new Error(`Failed to generate LLM response: ${error.message}`);
  }
};
```

This function does one thing: it takes some parameters, makes an API call, and returns the generated text. It doesn't need to remember anything between calls, and it doesn't have any internal state.

### Class Snippets

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
Here's our `NoteManager` class snippet:

```javascript
class NoteManager {
  constructor() {
    this.currentNote = null;
  }

  async getCurrentNote() {
    const leaves = app.workspace.getLeavesOfType("markdown");
    const activeLeaf = leaves
      .filter(leaf => leaf.activeTime > 0)
      .sort((a, b) => b.activeTime - a.activeTime)[0];
    
    if (activeLeaf && activeLeaf.view && activeLeaf.view.file) {
      return activeLeaf.view.file;
    }

    // Fallback to the most recent markdown file
    const files = app.vault.getMarkdownFiles();
    if (files.length === 0) {
      throw new Error("No Markdown files found in the vault.");
    }
    return files.sort((a, b) => b.stat.mtime - a.stat.mtime)[0];
  }

  async updateNote(content) {
    const noteToUpdate = await this.getCurrentNote();
    await app.vault.modify(noteToUpdate, content);
  }

  async createNote(name, content) {
    const newNote = await app.vault.create(name, content);
    return newNote;
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

4. **Organization**:
   - Function snippets are good for organizing unrelated utilities.
   - Class snippets are great for grouping related functionality.

5. **Flexibility**:
   - Function snippets are more flexible and can be easily composed.
   - Class snippets provide a more structured approach to complex tasks.

In practice, you'll likely use both types in your Obsistants Tools. Function snippets like `llmBlock` are great for discrete operations like API calls, while class snippets like our `NoteManager` are perfect for managing more complex, stateful operations.

Remember, the goal is to choose the right tool for the job. As you develop more Obsistants Tools, you'll get a feel for when to use each type of snippet. Don't be afraid to experiment!

## Using Snippets in Obsistants Tools

When building your Obsistants Tools, you can leverage both function and class snippets to enhance your tool's capabilities. Here's how to use them:

### Accessing Snippets

All snippets are available globally within your tool functions. There's no need to import or pass them as parameters.

### Using Function Snippets

Function snippets can be called directly. Here's how to use the `llmBlock` function snippet:

```javascript
const myTool = async ({ textToTranslate }) => {
  try {
    const result = await llmBlock({
      prompt: `Translate to French: ${textToTranslate}`,
      model: "#sonnet",
      maxTokens: 50
    });
    return `Translation: ${result}`;
  } catch (error) {
    console.error("Error in myTool:", error);
    return `Error: ${error.message}`;
  }
};
```

Key points:
- Call the function directly as it's globally available.
- Use `await` as most snippets are asynchronous.
- Always wrap snippet calls in a try/catch block for error handling.

### Using Class Snippets

Class snippets are instantiated once and their methods can be called directly. Here's an example with our `NoteManager` class snippet:

```javascript
const noteManager = new NoteManager();

const myNoteTool = async () => {
  try {
    const currentNote = await noteManager.getCurrentNote();
    const content = await app.vault.read(currentNote);
    return `Current note content: ${content.substring(0, 100)}...`;
  } catch (error) {
    console.error("Error in myNoteTool:", error);
    return `Error: ${error.message}`;
  }
};
```

Key points:
- Instantiate the class once, typically at the top of your tool file.
- Use the instance's methods as needed.

### Combining Snippets

You can use multiple snippets in a single tool. Here's an example that combines both types:

```javascript
const noteManager = new NoteManager();

const myCombinedTool = async () => {
  try {
    // Use NoteManager to get current note
    const currentNote = await noteManager.getCurrentNote();
    const content = await app.vault.read(currentNote);

    // Use llmBlock to summarize
    const summary = await llmBlock({
      prompt: `Summarize: ${content.substring(0, 500)}...`,
      model: "#sonnet",
      maxTokens: 100
    });

    // Create a new note with the summary
    await noteManager.createNote("Summary.md", summary);

    return `Summary created successfully.`;
  } catch (error) {
    console.error("Error in myCombinedTool:", error);
    return `Error: ${error.message}`;
  }
};
```

Remember:
1. Snippets are globally available within your tool functions.
2. Use `await` with asynchronous snippet functions.
3. Instantiate class snippets before using their methods.
4. Wrap snippet usage in try/catch blocks for error handling.
5. You can combine multiple snippets to create powerful, complex tools.

> [!important]
> Snippets must contain a `js` (or `javascript`) code block. If you write any of your code *outside of a code block*, it simply will not run!
> 
> Example:
> ~~~
> ```js
> const mySnippet = async (params) => {
>   // Your code here
> };
> ```
> ~~~


## Distinguishing Tools and Snippets

While both tools and snippets are key components in the Obsistants ecosystem, they are used in fundamentally different ways:

- **Tools** are invoked directly by your AI assistant during a chat interaction. When you request a specific function or task, the assistant can call the appropriate tool to perform that action.

- **Snippets**, on the other hand, are used within tools. They are reusable pieces of code that tools can leverage to perform common operations or complex tasks. Snippets are not directly accessible to the AI assistant or the user during a chat; instead, they serve as building blocks for the tools themselves.

Understanding this distinction is crucial for effectively developing and using Obsistants Tools and Snippets. Tools act as the interface between the user (via the AI assistant) and the Obsidian environment, while snippets provide the underlying functionality that tools can tap into.


## Creating Tools with AI Assistance

Developing Obsistants Tools can be significantly streamlined with the help of AI assistants. Here's how you can leverage AI to create powerful tools:

1. **Documentation as a Resource**: Providing this documentation to your AI assistant is an excellent starting point. AI can interpret and apply this information to help you craft well-structured tools.

2. **API Knowledge**: AI assistants typically have a good understanding of the Obsidian API. While not flawless, they can guide you in the right direction and suggest appropriate API calls for your tool's functionality.

3. **Code Generation**: Describe the functionality you want, and the AI can generate initial code structures or even complete tool implementations.

4. **Problem Solving**: When you're stuck on a particular feature or encountering issues, AI can offer solutions or alternative approaches.

5. **Best Practices**: AI can help ensure your tools follow best practices in terms of structure, error handling, and performance.

Remember, while AI is a powerful ally in tool creation, always review and test the generated code to ensure it meets your specific needs and works as intended within your Obsidian environment.

### Using the Developer Console

The developer console is an invaluable tool for exploring APIs and debugging your tools. Here are some key points to remember:

1. Any object you can access within the developer console is an object a tool or snippet can use!
2. Use `console.log()` statements in your code to help understand what's happening and to identify issues.
3. To open the developer console in Obsidian:
   - On Windows/Linux: Press `Ctrl + Shift + I`
   - On macOS: Press `Cmd + Option + I`

For a comprehensive guide on using Chrome's developer console (which is similar to the one in Obsidian), check out [Google's official documentation](https://developer.chrome.com/docs/devtools/console/).

## Troubleshooting and Getting Help

When developing tools, encountering errors is part of the process. Here's an expanded guide on troubleshooting:

1. **Understand the Error**: 
   - Read the error message carefully. It often contains vital clues about what's going wrong.
   - Look for specific line numbers, variable names, or function calls mentioned in the error.

2. **Use Console Logging**:
   - Implement `console.log()` statements strategically in your code.
   - Log variable values, function inputs/outputs, and execution flow to pinpoint where issues occur.
   - Example:
     ```javascript
     console.log(`Function called with parameters: ${JSON.stringify(params)}`);
     ```

3. **Leverage Developer Tools**:
   - Use Obsidian's developer console (Ctrl+Shift+I or Cmd+Option+I) to view logs and errors.
   - Set breakpoints in your code to pause execution and inspect the state at specific points.

4. **AI Assistant Diagnosis**:
   - Copy and paste error messages, relevant code snippets, or console logs to your AI assistant.
   - Provide context about what you were trying to achieve when the error occurred.
   - Ask specific questions about the error or for suggestions on how to resolve it.

5. **Iterative Testing**:
   - Make small, incremental changes and test after each modification.
   - This approach helps isolate the source of problems more easily.

6. **Community Resources**:
   - [Visit our Discord](https://discord.gg/w5Tz34WD), don't hesitate to ask for help, providing a clear description of your problem and what you've tried.

7. **Error Logs**:
   - For persistent issues, generate comprehensive error logs.
   - Include:
     - The full error message and stack trace
     - Relevant parts of your tool's code
     - Steps to reproduce the error
     - Your Obsidian and plugin versions
   - Share these logs with your AI assistant or the community for more targeted help.

## Obsidian API Documentation

While AI assistants are knowledgeable about the Obsidian API, always cross-reference with the official documentation:

- Refer to the [official Obsidian API documentation](https://github.com/obsidianmd/obsidian-api) for the most up-to-date and comprehensive information.
- The API evolves over time, so check for recent updates or changes that might affect your tools.
- If you notice discrepancies between AI suggestions and the official documentation, prioritize the official source.

Remember, creating tools is an iterative process. Don't be discouraged by initial setbacks – with persistence, experimentation, and the support of AI and the community, you can create powerful and effective Obsistants Tools that significantly enhance your Obsidian workflow.
