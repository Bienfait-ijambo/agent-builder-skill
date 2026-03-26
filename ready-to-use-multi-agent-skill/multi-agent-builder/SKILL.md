---
name: multi-agent-builder
description: Generate Supervisor-based Multi-Agent System JSON for React Flow canvas. Use this skill when the user wants to build AI agent systems, multi-agent architectures, supervisor-worker patterns, agent orchestration, or needs JSON output for agent flow visualization. Triggers on phrases like "build an agent", "create multi-agent system", "agent builder", "supervisor agent", "worker agents", "agent workflow", or when describing agent capabilities and hierarchies.
---

# Multi-Agent Builder Skill

A skill for generating Supervisor-based Multi-Agent System JSON that can be rendered in React Flow canvas.

## Overview

This skill enables Claude to generate structured JSON representing a multi-agent system with:
- **Tree structure** (`children`) for execution hierarchy
- **Graph structure** (`referenceTo`) for node relationships
- **Supervisor pattern** where a Manager Agent orchestrates Worker Agents (subAgents)

## When to Use This Skill

Use this skill when:
- User wants to build an AI agent system
- User describes a workflow with multiple specialized agents
- User needs agent orchestration with tools and models
- User wants JSON output for React Flow visualization
- User mentions "supervisor agent", "worker agents", "multi-agent", "agent hierarchy"

## ⚠️ Clarification Protocol (MANDATORY)

**IMPORTANT:** When a user prompts you to create an agent, **DO NOT immediately generate the JSON**. You MUST first ask clarifying questions to fully understand the user's requirements.

### Why Clarification Matters
Creating a multi-agent system without proper understanding can lead to:
- Incorrect agent hierarchy
- Missing or unnecessary tools
- Poorly defined agent roles and instructions
- Inefficient or broken workflows

### Required Clarification Questions

Before generating any JSON, ask the user the following questions (at minimum):

1. **Purpose & Goal**
   - What is the main purpose of this agent system?
   - What problem should it solve for the user?

2. **Agent Structure**
   - Do you need a single agent or multiple agents working together?
   - If multiple agents, should there be a manager/supervisor coordinating workers?

3. **Tools & Capabilities**
   - What tools or capabilities does each agent need?
   - Should agents have access to web search, file operations, image generation, etc.?

4. **Models**
   - Do you want to specify particular AI models for any agents?
   - Are there performance requirements that need specific model choices?

5. **Workflow Details**
   - How should agents interact with each other?
   - Are there specific tasks that should be delegated to specific agents?

### When You Can Skip Clarification

You may proceed directly to JSON generation ONLY when:
- The user explicitly says "create exactly what I described" or similar
- The user provides a complete, detailed specification including:
  - Agent names and roles
  - Tool assignments
  - Model selections
  - Hierarchy structure
- The user is asking for a modification to an existing system you just created

### Example Clarification Exchange

**User:** "Create a research agent"

**Agent (DO NOT generate JSON yet):**
> I'd be happy to help you create a research agent! Before I design the system, I need to understand your needs better:
>
> 1. What kind of research should this agent perform? (web research, document analysis, both?)
> 2. Do you need a single agent or a team with a manager coordinating specialized researchers?
> 3. What tools would you like the agent(s) to have access to? (web search, document retriever, etc.)
> 4. Do you have a preferred AI model for this system?
> 5. What's the main goal or output you expect from this research system?

**User:** "I want a manager that coordinates a web researcher and a document analyzer, using GPT-4 for the manager and GPT-3.5 for workers"

**Agent (NOW generate JSON):**
> Perfect! Now I have a clear picture. I'll create a multi-agent system with:
> - A Research Manager (GPT-4) coordinating two workers
> - A Web Researcher with web search capability
> - A Document Analyzer with retriever capability
> Both workers using GPT-3.5 as specified.

*[Proceed to generate JSON]*

## Core Concepts

### Node Types

| Node Type | Role | Can Contain |
|-----------|------|-------------|
| `inputNode` | Entry point (always root) | agent |
| `agent` | Manager/Supervisor | tool, modelNode, subAgent |
| `subAgent` | Specialized worker | tool, modelNode, subAgent |
| `tool` | Executable capability | (empty - no children) |
| `modelNode` | AI model attachment | (empty - no children) |

### Node Registry Types

> ⚠️ **CRITICAL VALIDATION RULE**: The `nodeRegistry` field MUST ONLY contain values from this list. Any tool passed to an agent MUST have a `nodeRegistry` value from the 19 valid types below. **REJECT or WARN about invalid tool types not in this list.**

#### Complete NodeRegistryType Definition

```typescript
export type NodeRegistryType =
  | "search"
  | "webscraper"
  | "retriever"
  | "memory"
  | "model"
  | "sheet"
  | "readSheet"
  | "pieChart"
  | "lineChart"
  | "vectorDB"
  | "embeddingModel"
  | "sendMail"
  | "calendarEvent"
  | "readFile"
  | "writeFile"
  | "readAndUpdateFile"
  | "imageGenerator"
  | "imageReader"
  | "imageEditor";
```

#### Tool Registry Descriptions

| Tool | Description | Use Cases |
|------|-------------|-----------|
| **search** | Performs real-time web search to retrieve relevant information | Finding up-to-date information, research queries, gathering external knowledge |
| **webscraper** | Extracts structured content from web pages (HTML parsing, DOM extraction) | Scraping articles, extracting data from websites, parsing structured content |
| **retriever** | Retrieves relevant documents from a knowledge base using similarity search | RAG (Retrieval-Augmented Generation), document search, context injection for LLMs |
| **memory** | Stores and retrieves conversation or system memory | Chat history, long-term context, personalization |
| **model** | Defines the AI model used by an agent for reasoning and generation | Text generation, decision making, planning and reasoning |
| **sheet** | Creates or writes structured data into a spreadsheet | Logging results, generating reports, data organization |
| **readSheet** | Reads data from an existing spreadsheet | Data analysis, fetching stored records, feeding structured input to agents |
| **pieChart** | Generates a pie chart visualization from structured data | Data visualization, reports and dashboards |
| **lineChart** | Generates a line chart for time-series or trend data | Trend analysis, performance tracking |
| **vectorDB** | Stores and queries vector embeddings for semantic search | RAG systems, semantic similarity search, knowledge storage |
| **embeddingModel** | Converts text into vector embeddings | Indexing documents, semantic search, similarity comparison |
| **sendMail** | Sends emails via a configured provider | Notifications, reports delivery, user communication |
| **calendarEvent** | Creates or manages calendar events | Scheduling meetings, reminders, task planning |
| **readFile** | Reads content from a file | Processing documents, loading data, code analysis |
| **writeFile** | Creates or writes content to a file | Saving outputs, generating reports, code generation |
| **readAndUpdateFile** | Reads a file, modifies its content, and writes updates back | Editing code, updating documents, incremental changes |
| **imageGenerator** | Generates images from text prompts using AI models | Design assets, visual content creation, UI mockups |
| **imageReader** | Analyzes and extracts information from images | OCR (text extraction), image understanding, screenshot analysis |
| **imageEditor** | Modifies or enhances existing images | Image transformations, editing visuals, style changes |

#### Validation Rules

1. **ONLY these 19 tool types are valid** for the `nodeRegistry` field
2. **DO NOT** create tools with `nodeRegistry` values not in this list
3. **WARN the user** if they request a tool type that doesn't exist
4. **Suggest alternatives** if a requested capability isn't available

**Example of Invalid Tool (REJECT THIS):**
```json
{
  "nodeRegistry": "mindMap",  // ❌ INVALID - not in NodeRegistryType
  ...
}
```

**Valid Alternative:**
```json
{
  "nodeRegistry": "imageGenerator",  // ✅ VALID - can generate visual diagrams
  ...
}
```

## 🎨 Icon Usage Instructions (MANDATORY)

> ⚠️ **CRITICAL**: Every node MUST have a valid `icon` field in its `config` object. The icon path MUST reference a file that exists in the `icons/` folder.

### Icon Registry Location

All icons are located at: `skills/multi-agent-builder/icons/`

When referencing icons in JSON config, use the path format: `/icons/<filename>`

### Icon Mapping by Node Type

| Node Type | Recommended Icon | Path |
|-----------|------------------|------|
| `inputNode` | input.png | `/icons/input.png` |
| `agent` | bot.png | `/icons/bot.png` |
| `subAgent` | bot.png | `/icons/bot.png` |

### Icon Mapping by Tool (nodeRegistry)

| nodeRegistry | Recommended Icon | Path |
|--------------|------------------|------|
| `search` | search.png | `/icons/search.png` |
| `webscraper` | crawler.png | `/icons/crawler.png` |
| `retriever` | retriever.png | `/icons/retriever.png` |
| `memory` | message.png | `/icons/message.png` |
| `model` | (provider-specific) | See below |
| `sheet` | sheet.png | `/icons/sheet.png` |
| `readSheet` | sheet.png | `/icons/sheet.png` |
| `pieChart` | pie.png | `/icons/pie.png` |
| `lineChart` | chart.png | `/icons/chart.png` |
| `vectorDB` | db.png | `/icons/db.png` |
| `embeddingModel` | embedding.png | `/icons/embedding.png` |
| `sendMail` | gmail.png | `/icons/gmail.png` |
| `calendarEvent` | calendar.png | `/icons/calendar.png` |
| `readFile` | read_file.png | `/icons/read_file.png` |
| `writeFile` | write_file.ico | `/icons/write_file.ico` |
| `readAndUpdateFile` | edit_file.png | `/icons/edit_file.png` |
| `imageGenerator` | pic.png | `/icons/pic.png` |
| `imageReader` | camera.png | `/icons/camera.png` |
| `imageEditor` | image_editor.png | `/icons/image_editor.png` |

### Icon Mapping by Model Provider (for modelNode)

| Provider | Icon | Path |
|----------|------|------|
| OpenAI (GPT-4, GPT-3.5) | openai-icon.svg | `/icons/openai-icon.svg` |
| Google Gemini | gemini.png | `/icons/gemini.png` |
| DeepSeek | deepseek-icon.svg | `/icons/deepseek-icon.svg` |
| Meta Llama | meta-icon.svg | `/icons/meta-icon.svg` |
| Mistral | mistral-ai-icon.svg | `/icons/mistral-ai-icon.svg` |
| Qwen | qwen-icon.svg | `/icons/qwen-icon.svg` |
| Kimi | kimi.svg | `/icons/kimi.svg` |
| Unknown/Fallback | default.png | `/icons/default.png` |

### Icon Validation Rules

1. **Every node MUST have an `icon` field** - never leave it empty
2. **Icon paths MUST match actual files** - verify the file exists in `icons/` folder
3. **Use correct file extension case** - most icons use lowercase `.png` or `.svg`
4. **Match icon to node type** - use the appropriate icon for each tool type

### Common Icon Mistakes

```json
// ❌ WRONG: Non-existent icon
"icon": "/icons/weather.PNG"

// ❌ WRONG: Empty icon
"icon": ""

// ❌ WRONG: Wrong extension case
"icon": "/icons/search.PNG"  // File is search.png

// ✅ CORRECT: Valid icon with correct case
"icon": "/icons/search.png"
```

> 📁 **Full Icon Reference**: See `references/icon-registry.md` for complete icon inventory and detailed usage examples.

---

## 🤖 Available Models Registry (MANDATORY)

> ⚠️ **CRITICAL**: When adding a model node to an agent, you MUST use one of the models listed below. The `model` field in the modelNode MUST match the `model` value from this registry. Do NOT assume a default model - always ask the user which model they want to use.

### Model Clarification Protocol

**When building an agent that includes a model node, you MUST ask the user which specific model they want to use.**

#### How to Ask:
1. Present the available options organized by provider
2. Show both the model name and provider
3. Do NOT assume a default model
4. Wait for user's explicit choice before generating JSON

#### Example Clarification Exchange:

**User:** "Create a research agent with a model"

**Agent (DO NOT generate JSON yet):**
> I'd be happy to create a research agent! Before I proceed, I need to know which AI model you'd like to use.
>
> **Available Models:**
>
> **Fireworks Provider:**
> - `deepseek-v3p1` - DeepSeek V3.1 (high performance)
> - `kimi-k2-instruct-0905` - Kimi K2 Instruct
> - `kimi-k2-thinking` - Kimi K2 Thinking (reasoning model)
> - `deepseek-v3p1-terminus` - DeepSeek V3.1 Terminus
> - `gpt-oss-120b` - GPT-OSS 120B (OpenAI compatible)
>
> **Cerebras Provider:**
> - `llama-3.3-70b` - Meta Llama 3.3 70B
> - `gpt-oss-120b` - GPT-OSS 120B (OpenAI compatible)
> - `qwen-3-32b` - Qwen 3 32B
>
> **Google Provider:**
> - `gemini-2.5-pro` - Google Gemini 2.5 Pro (most capable)
> - `gemini-2.5-flash` - Google Gemini 2.5 Flash (fast)
>
> Which model would you like to use?

**User:** "Use gemini-2.5-pro"

**Agent (NOW generate JSON):**
> Perfect! I'll use Gemini 2.5 Pro for your research agent.

*[Proceed to generate JSON with correct model ID]*

---

### Complete Model Registry

#### CHAT_FIREWORKS Provider

| Model Name | Model ID (use in `model` field) | Icon | Description |
|------------|-------------------------------|------|-------------|
| DeepSeek V3.1 | `accounts/fireworks/models/deepseek-v3p1` | `/icons/deepseek-icon.svg` | High-performance reasoning model |
| Kimi K2 Instruct | `accounts/fireworks/models/kimi-k2-instruct-0905` | `/icons/kimi.svg` | Kimi instruction-following model |
| Kimi K2 Thinking | `accounts/fireworks/models/kimi-k2-thinking` | `/icons/kimi.svg` | Kimi reasoning/thinking model |
| DeepSeek V3.1 Terminus | `accounts/fireworks/models/deepseek-v3p1-terminus` | `/icons/deepseek-icon.svg` | DeepSeek Terminus variant |
| GPT-OSS 120B | `accounts/fireworks/models/gpt-oss-120b` | `/icons/openai-icon.svg` | OpenAI-compatible large model |

#### CHAT_CEREBRAS Provider

| Model Name | Model ID (use in `model` field) | Icon | Description |
|------------|-------------------------------|------|-------------|
| Llama 3.3 70B | `llama-3.3-70b` | `/icons/meta-icon.svg` | Meta's Llama 3.3 70B model |
| GPT-OSS 120B | `gpt-oss-120b` | `/icons/openai-icon.svg` | OpenAI-compatible large model |
| Qwen 3 32B | `qwen-3-32b` | `/icons/qwen-icon.svg` | Alibaba's Qwen 3 32B model |

#### GOOGLE Provider

| Model Name | Model ID (use in `model` field) | Icon | Description |
|------------|-------------------------------|------|-------------|
| Gemini 2.5 Pro | `gemini-2.5-pro` | `/icons/gemini.png` | Google's most capable Gemini model |
| Gemini 2.5 Flash | `gemini-2.5-flash` | `/icons/gemini.png` | Google's fast Gemini model |

---

### Model Node JSON Format

When adding a model node, use this format:

```json
{
  "node_name": "modelNode",
  "referenceTo": ["<parent_agent_or_subagent>"],
  "config": {
    "label": "<Model Name>",
    "icon": "<icon_path_from_registry>",
    "model": "<model_id_from_registry>",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": "<provider_name>"
    }
  },
  "children": []
}
```

### Example Model Nodes

#### Using DeepSeek V3.1 (Fireworks)
```json
{
  "node_name": "modelNode",
  "referenceTo": ["agent"],
  "config": {
    "label": "DeepSeek V3.1",
    "icon": "/icons/deepseek-icon.svg",
    "model": "accounts/fireworks/models/deepseek-v3p1",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": "CHAT_FIREWORKS"
    }
  },
  "children": []
}
```

#### Using Gemini 2.5 Pro (Google)
```json
{
  "node_name": "modelNode",
  "referenceTo": ["agent"],
  "config": {
    "label": "Gemini 2.5 Pro",
    "icon": "/icons/gemini.png",
    "model": "gemini-2.5-pro",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": "GOOGLE"
    }
  },
  "children": []
}
```

#### Using Llama 3.3 70B (Cerebras)
```json
{
  "node_name": "modelNode",
  "referenceTo": ["agent"],
  "config": {
    "label": "Llama 3.3 70B",
    "icon": "/icons/meta-icon.svg",
    "model": "llama-3.3-70b",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": "CHAT_CEREBRAS"
    }
  },
  "children": []
}
```

### Model Validation Rules

1. **ONLY use models from this registry** - Do NOT create models with IDs not listed above
2. **Always ask for clarification** - Never assume a default model
3. **Match icon to model provider** - Use the correct icon for each model
4. **Use exact model ID** - The `model` field must match exactly (case-sensitive)
5. **Specify correct provider** - The `config.provider` must match the provider group

### Common Model Mistakes

```json
// ❌ WRONG: Invalid model ID
"model": "gpt-4"  // Not in registry

// ❌ WRONG: Assuming default without asking
// (You asked the user which model to use)

// ❌ WRONG: Wrong icon for provider
"model": "gemini-2.5-pro",
"icon": "/icons/openai-icon.svg"  // Should be gemini.png

// ✅ CORRECT: Valid model with correct icon and provider
"model": "gemini-2.5-pro",
"icon": "/icons/gemini.png",
"config": { "provider": "GOOGLE" }
```

> 📁 **Full Model Reference**: See `references/model-registry.md` for complete model registry with detailed descriptions and usage examples.

---

## Rules (MUST Follow)

1. **System MUST start with `inputNode`** - it's always the root
2. **`inputNode.referenceTo` MUST be empty array `[]`**
3. **`tool.children` MUST be empty array `[]`**
4. **`subAgent.children` can contain**: tool, modelNode, subAgent
5. **`agent.children` can contain**: tool, modelNode, subAgent
6. **`referenceTo` defines node links** - reference parent node names
7. **`instructions` field contains the system prompt for agents/subAgents**
8. **`description` field describes what the agent/tool does**
9. **`icon` field MUST reference a valid icon** from the `icons/` folder

---

## ⚠️ CRITICAL: `referenceTo` Field Validation Rule

> **THIS IS THE MOST COMMON ERROR IN MULTI-AGENT JSON GENERATION. READ THIS CAREFULLY.**

### The Rule

**The `referenceTo` field MUST ALWAYS reference the `node_name` field value, NEVER the `label` field value.**

### Why This Matters

The `referenceTo` field creates graph edges between nodes. The system uses `node_name` to identify nodes programmatically. Using `label` (which is a human-readable display name) will break the graph structure.

### Correct `referenceTo` Values by Parent

| Child Node | Parent Node | Correct `referenceTo` |
|------------|-------------|----------------------|
| `agent` | `inputNode` | `["inputNode"]` |
| `tool` (under agent) | `agent` | `["agent"]` |
| `subAgent` (under agent) | `agent` | `["agent"]` |
| `modelNode` (under agent) | `agent` | `["agent"]` |
| `tool` (under subAgent) | `subAgent` | `["subAgent"]` |
| `modelNode` (under subAgent) | `subAgent` | `["subAgent"]` |
| `subAgent` (under subAgent) | `subAgent` | `["subAgent"]` |

### Common Mistake Examples

```json
// ❌ WRONG: Using label instead of node_name
{
  "node_name": "tool",
  "referenceTo": ["Research Manager"],  // WRONG! This is the label!
  "config": {
    "label": "Web Search",
    ...
  }
}

// ❌ WRONG: Using label for subAgent's children
{
  "node_name": "modelNode",
  "referenceTo": ["Web Search Agent"],  // WRONG! This is the label!
  "config": {
    "label": "DeepSeek V3.1",
    ...
  }
}

// ✅ CORRECT: Using node_name for agent's children
{
  "node_name": "tool",
  "referenceTo": ["agent"],  // CORRECT! This is the node_name!
  "config": {
    "label": "Web Search",
    ...
  }
}

// ✅ CORRECT: Using node_name for subAgent's children
{
  "node_name": "modelNode",
  "referenceTo": ["subAgent"],  // CORRECT! This is the node_name!
  "config": {
    "label": "DeepSeek V3.1",
    ...
  }
}
```

### Validation Checklist

Before finalizing any JSON, verify:

- [ ] All `referenceTo` values use `node_name` (e.g., `"agent"`, `"subAgent"`, `"inputNode"`)
- [ ] NO `referenceTo` values use `label` (e.g., `"Research Manager"`, `"Web Search Agent"`)
- [ ] Tools under agent have `"referenceTo": ["agent"]`
- [ ] SubAgents under agent have `"referenceTo": ["agent"]`
- [ ] Tools under subAgent have `"referenceTo": ["subAgent"]`
- [ ] ModelNodes under subAgent have `"referenceTo": ["subAgent"]`

### Quick Reference

| Node Type | `node_name` Value | `label` Example | Use in `referenceTo`? |
|-----------|-------------------|-----------------|----------------------|
| Input | `"inputNode"` | `"Input"` | ✅ YES |
| Agent | `"agent"` | `"Research Manager"` | ✅ YES |
| SubAgent | `"subAgent"` | `"Web Search Agent"` | ✅ YES |
| Tool | `"tool"` | `"Web Search"` | ❌ NO (tools don't have children) |
| ModelNode | `"modelNode"` | `"DeepSeek V3.1"` | ❌ NO (models don't have children) |

**Remember**: `node_name` is the programmatic identifier. `label` is just for display. Always use `node_name` in `referenceTo`!

## JSON Structure Template

### Input Node (Required - Always First)
```json
{
  "node_name": "inputNode",
  "referenceTo": [],
  "config": {
    "label": "When chat message received",
    "icon": "/icons/input.png",
    "ui": {}
  },
  "children": []
}
```

### Agent Node (Manager/Supervisor)
```json
{
  "node_name": "agent",
  "referenceTo": ["inputNode"],
  "config": {
    "label": "Manager Agent",
    "icon": "/icons/bot.png",
    "sub": "Tools Agent",
    "instructions": "<system prompt for the manager>",
    "description": "<what this agent does>"
  },
  "children": []
}
```

### SubAgent Node (Worker)
```json
{
  "node_name": "subAgent",
  "referenceTo": ["agent"],
  "config": {
    "label": "Worker Agent",
    "icon": "/icons/bot.png",
    "sub": "worker Agent",
    "instructions": "<system prompt for this worker>",
    "description": "<what this worker does>",
    "model": ""
  },
  "children": []
}
```

### Tool Node
```json
{
  "node_name": "tool",
  "referenceTo": ["<parent_node_name>"],
  "config": {
    "label": "Tool Name",
    "icon": "/icons/search.png",
    "nodeRegistry": "<registry_type>",
    "name": "Tool Name",
    "config": {
      "provider": "",
      "description": "<what this tool does>"
    }
  },
  "children": []
}
```

### Model Node
```json
{
  "node_name": "modelNode",
  "referenceTo": ["<parent_node_name>"],
  "config": {
    "label": "<Model_Display_Name>",
    "icon": "<icon_path_from_model_registry>",
    "model": "<exact_model_id_from_registry>",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": "<CHAT_FIREWORKS|CHAT_CEREBRAS|GOOGLE>"
    }
  },
  "children": []
}
```

> ⚠️ **IMPORTANT**: See the "Available Models Registry" section above for valid model IDs, icons, and providers. Always ask the user which model they want to use before adding a model node.

## Generation Workflow

When generating a multi-agent system JSON:

### Step 1: Analyze User Request
- Identify the main purpose of the agent system
- Determine if it needs a supervisor pattern
- List required tools and capabilities
- Identify if multiple specialized workers are needed

### Step 2: Design the Architecture
- Define the Manager Agent's role and system prompt
- Determine if subAgents are needed for specialization
- Map tools to the appropriate agent/subAgent
- Assign models if specific AI models are required

### Step 3: Generate the JSON
1. Always start with `inputNode`
2. Add the main `agent` (Manager) as child of inputNode
3. Add tools, modelNode, and subAgents to the agent's children
4. For each subAgent, add their tools and models
5. Ensure all `referenceTo` fields correctly point to parent nodes

### Step 4: Validate
- Check that inputNode is root with empty referenceTo
- Verify tool nodes have empty children
- Confirm all referenceTo values match existing node names
- Ensure instructions and descriptions are meaningful

## Output Format

Always output the complete JSON array. The output should be:
- Valid JSON syntax
- Properly nested according to tree structure
- Include meaningful instructions and descriptions
- Ready for React Flow canvas rendering

## Examples

### Example 1: Simple Agent with Search Tool
User: "Create an agent that can search the web"

```json
[
  {
    "node_name": "inputNode",
    "referenceTo": [],
    "config": {
      "label": "When chat message received",
      "icon": "/icons/input.png",
      "ui": {}
    },
    "children": [
      {
        "node_name": "agent",
        "referenceTo": ["inputNode"],
        "config": {
          "label": "Search Agent",
          "icon": "/icons/bot.png",
          "sub": "Tools Agent",
          "instructions": "You are a helpful assistant that can search the web to answer user questions.",
          "description": "An agent that helps users by searching the web for information."
        },
        "children": [
          {
            "node_name": "tool",
            "referenceTo": ["agent"],
            "config": {
              "label": "Web Search",
              "icon": "/icons/search.png",
              "nodeRegistry": "search",
              "name": "Web Search",
              "config": {
                "provider": "",
                "description": "Search the web for information"
              }
            },
            "children": []
          }
        ]
      }
    ]
  }
]
```

### Example 2: Research Team with Multiple Workers (with Workspace Cleaner Pre-step)
User: "Build a research team with a manager that coordinates a web researcher and a document analyzer"

> ⚠️ **IMPORTANT**: For research-related workflows, always include `workspace-cleaner` as a pre-step tool to ensure a clean working environment before research tasks begin.

```json
[
  {
    "node_name": "inputNode",
    "referenceTo": [],
    "config": {
      "label": "When chat message received",
      "icon": "/icons/input.png",
      "ui": {}
    },
    "children": [
      {
        "node_name": "agent",
        "referenceTo": ["inputNode"],
        "config": {
          "label": "Research Manager",
          "icon": "/icons/bot.png",
          "sub": "Tools Agent",
          "instructions": "You are a research manager. Coordinate between the web researcher and document analyzer to provide comprehensive research results. Delegate tasks appropriately based on the type of information needed.",
          "description": "Manages a team of specialized research agents"
        },
        "children": [
          {
            "node_name": "tool",
            "referenceTo": ["agent"],
            "config": {
              "label": "Workspace Cleaner",
              "icon": "/icons/file.svg",
              "nodeRegistry": "writeFile",
              "name": "Workspace Cleaner",
              "config": {
                "provider": "",
                "description": "Cleans workspace before research tasks - removes old files, creates fresh working directory structure"
              }
            },
            "children": []
          },
          {
            "node_name": "subAgent",
            "referenceTo": ["agent"],
            "config": {
              "label": "Web Researcher",
              "icon": "/icons/bot.png",
              "sub": "worker Agent",
              "instructions": "You are a web research specialist. Use web search to find up-to-date information on any topic. Provide detailed and accurate findings.",
              "description": "Searches the web for current information"
            },
            "children": [
              {
                "node_name": "tool",
                "referenceTo": ["subAgent"],
                "config": {
                  "label": "Web Search",
                  "icon": "/icons/search.png",
                  "nodeRegistry": "search",
                  "name": "Web Search",
                  "config": {
                    "provider": "",
                    "description": "Search the web for information"
                  }
                },
                "children": []
              }
            ]
          },
          {
            "node_name": "subAgent",
            "referenceTo": ["agent"],
            "config": {
              "label": "Document Analyzer",
              "icon": "/icons/bot.png",
              "sub": "worker Agent",
              "instructions": "You are a document analysis specialist. Use the retriever to find relevant information from documents and provide detailed analysis.",
              "description": "Analyzes documents for relevant information"
            },
            "children": [
              {
                "node_name": "tool",
                "referenceTo": ["subAgent"],
                "config": {
                  "label": "Document Retriever",
                  "icon": "/icons/retriever.png",
                  "nodeRegistry": "retriever",
                  "name": "Retriever",
                  "config": {
                    "provider": "",
                    "description": "Retrieve information from documents"
                  }
                },
                "children": []
              }
            ]
          }
        ]
      }
    ]
  }
]
```

> 📝 **Note on `referenceTo`**: In this example, notice how the workspace-cleaner tool uses `"referenceTo": ["agent"]` (the `node_name`), NOT `"Research Manager"` (the `label`). This is critical for proper graph structure.

## Tips for Good Output

1. **Clear Instructions**: Write detailed system prompts for each agent/subAgent
2. **Meaningful Descriptions**: Describe what each component does
3. **Logical Hierarchy**: Group related tools under the same agent/subAgent
4. **Appropriate Tools**: Match tools to the agent's purpose
5. **Model Assignment**: Assign specific models when performance matters

## Common Patterns

### Pattern 1: Single Agent with Tools
```
inputNode → agent → [tool1, tool2, ...]
```

### Pattern 2: Supervisor with Workers
```
inputNode → agent → [subAgent1, subAgent2, ...]
                    ↓
              subAgent → [tools]
```

### Pattern 3: Full Stack
```
inputNode → agent → [modelNode, subAgent, tools]
                         ↓
                    subAgent → [modelNode, tools]
```

---

## 🧹 Workspace Cleaner in Research Workflows

> ⚠️ **CRITICAL RULE**: When using `referenceTo` field, you MUST use the `node_name` value (e.g., "agent", "subAgent", "tool") NOT the `label` (e.g., "Research Manager", "Workspace Cleaner"). This is a common error that must be avoided.

### Why Workspace Cleaner?

For research-related workflows, having a clean workspace before starting research tasks is essential:
- Removes stale data from previous sessions
- Creates fresh directory structure for new research
- Ensures clean file outputs and prevents data corruption
- Provides a consistent starting point for each research task

### When to Include Workspace Cleaner

Include `workspace-cleaner` as a pre-step tool when:
1. Building any research-related agent configuration
2. Creating data analysis workflows
3. Setting up document processing pipelines
4. Any workflow that generates files or collects data

### Workspace Cleaner Configuration

```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Workspace Cleaner",
    "icon": "/icons/file.svg",
    "nodeRegistry": "writeFile",
    "name": "Workspace Cleaner",
    "config": {
      "provider": "",
      "description": "Cleans workspace before research tasks - removes old files, creates fresh working directory structure"
    }
  },
  "children": []
}
```

### Key Points About `referenceTo`

| Field | Correct Value | Wrong Value |
|-------|---------------|-------------|
| `node_name` | `"agent"`, `"subAgent"`, `"tool"` | (not used in node_name) |
| `label` | `"Research Manager"`, `"Workspace Cleaner"` | (not used in referenceTo) |

**The `referenceTo` field ALWAYS uses `node_name`, never `label`.**

### Validation Reminder

Before finalizing any JSON with workspace-cleaner:
- [ ] Workspace cleaner tool has `"referenceTo": ["agent"]` (using node_name)
- [ ] NOT `"referenceTo": ["Workspace Cleaner"]` (this is the label - WRONG!)
- [ ] Workspace cleaner is positioned BEFORE research subAgents in the children array

## 🎯 Model Selection Strategy (Multi-Model Distribution)

> ⚠️ **IMPORTANT**: When building multi-agent systems, **DO NOT use the same model for all agents**. Different agents have different requirements, and intelligent model distribution improves performance, cost-efficiency, and output quality.

### Why Multi-Model Distribution Matters

1. **Performance Optimization** - Match model capabilities to task requirements
2. **Cost Efficiency** - Use lighter models for simpler tasks
3. **Quality Improvement** - Specialized models for specialized tasks
4. **Resource Management** - Balance computational load across providers

### Model Selection by Agent Type

| Agent Type | Recommended Models | Reasoning |
|------------|-------------------|-----------|
| **Orchestrator/Manager** | `gemini-2.5-pro`, `kimi-k2-thinking` | Strong reasoning, planning, coordination capabilities |
| **Code Generator** | `deepseek-v3p1`, `deepseek-v3p1-terminus` | Excellent code generation, structured output |
| **Data Analyst** | `gemini-2.5-pro`, `deepseek-v3p1` | Complex data interpretation, pattern recognition |
| **Visualization Agent** | `deepseek-v3p1`, `llama-3.3-70b` | Code generation for charts, visual design |
| **Email/Communication** | `gemini-2.5-flash`, `llama-3.3-70b` | Fast, efficient, good language processing |
| **Research Agent** | `gemini-2.5-pro`, `kimi-k2-instruct-0905` | Information synthesis, comprehension |
| **Simple Task Agent** | `gemini-2.5-flash`, `llama-3.3-70b` | Fast response, cost-effective |

### Model Capability Matrix

| Model | Reasoning | Code Gen | Speed | Cost | Best For |
|-------|-----------|----------|-------|------|----------|
| `gemini-2.5-pro` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | $$ | Complex orchestration, analysis |
| `gemini-2.5-flash` | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | $ | Fast tasks, communication |
| `deepseek-v3p1` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | $ | Code generation, visualization |
| `kimi-k2-thinking` | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | $ | Deep reasoning, planning |
| `llama-3.3-70b` | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | $ | Fast general-purpose tasks |
| `qwen-3-32b` | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | $ | Lightweight tasks |

### Multi-Model Distribution Rules

1. **Minimum 2-3 Different Models** - Never use a single model for all agents in a multi-agent system
2. **Orchestrator Gets Strongest Model** - The coordinator needs the best reasoning capabilities
3. **Workers Match Task Requirements** - Select models based on what the worker actually does
4. **Simple Tasks Use Fast Models** - Don't waste powerful models on simple formatting/sending tasks
5. **Balance Provider Usage** - Distribute across 2-3 providers for reliability

### Example Multi-Model Distribution

**Market Research System:**
```
Orchestrator (gemini-2.5-pro) → Strong reasoning for coordination
├── Researcher (deepseek-v3p1) → Good synthesis and analysis
├── Chart Generator (deepseek-v3p1) → Excellent code generation
└── Email Sender (gemini-2.5-flash) → Fast, efficient delivery
```

**Content Creation System:**
```
Orchestrator (kimi-k2-thinking) → Deep thinking for strategy
├── Writer (gemini-2.5-pro) → High-quality content generation
├── Editor (llama-3.3-70b) → Fast review and corrections
└── Publisher (gemini-2.5-flash) → Quick formatting and posting
```

### Model Selection Clarification Protocol

When building a multi-agent system, ask the user:

1. **"What type of agents do you need?"** - Identify orchestrator vs workers
2. **"What tasks will each agent perform?"** - Match capabilities to tasks
3. **"Do you have performance requirements?"** - Speed vs quality tradeoffs
4. **"What's your budget consideration?"** - Premium vs cost-effective models

**Example Clarification:**
> I see you need a multi-agent system with an orchestrator and 3 workers. To optimize performance, I recommend:
>
> - **Orchestrator**: `gemini-2.5-pro` (strong reasoning for coordination)
> - **Chart Generators**: `deepseek-v3p1` (excellent code generation)
> - **Email Sender**: `gemini-2.5-flash` (fast, efficient)
>
> This gives you 3 different models across 2 providers. Does this distribution work for you?

---

## 📝 System Prompt Best Practices

> ⚠️ **CRITICAL**: Quality system prompts are essential for reliable agent behavior. Use the `system-prompt-engineer` skill for crafting detailed prompts.

### System Prompt Anatomy

Every agent should have a comprehensive system prompt that includes:

1. **Role Definition** - Who/what the agent is
2. **Core Responsibilities** - Specific tasks and duties
3. **Input/Output Specifications** - Data formats expected and produced
4. **Tool Utilization** - How to use available tools
5. **Error Handling** - How to handle edge cases and failures
6. **Quality Standards** - What "good" output looks like
7. **Constraints** - What the agent should NOT do
8. **Integration Context** - How the agent fits in the larger system

### Prompt Quality Checklist

Before finalizing any agent's `instructions` field:

- [ ] Is the role clearly defined?
- [ ] Are responsibilities specific and actionable?
- [ ] Are input/output formats specified with examples?
- [ ] Is error handling documented?
- [ ] Are quality standards explicit?
- [ ] Are constraints (do's and don'ts) clear?
- [ ] Is the integration context explained?

### Example Quality System Prompt Structure

```text
You are the [Agent Name], a [specialist type] agent within the [system name].

## Role Definition
[Clear description of identity and expertise level]

## Core Responsibilities
### 1. [Primary Task]
- [Specific action 1]
- [Specific action 2]

### 2. [Secondary Task]
- [Specific action 1]
- [Specific action 2]

## Input/Output Specifications
### Expected Input Format:
```json
{ "example": "format" }
```

### Output Specifications:
- [Requirement 1]
- [Requirement 2]

## Error Handling Procedures
### [Error Type 1]
- [How to handle]

## Quality Standards
### [Quality Category]
- [Standard 1]
- [Standard 2]

## Constraints
### Do NOT:
- [Prohibition 1]
- [Prohibition 2]

### Do:
- [Requirement 1]
- [Requirement 2]

## Integration Context
[How this agent fits in the larger system]
```

---

## Notes

- The JSON output is designed for React Flow canvas rendering
- Each node has a position that will be calculated by the canvas
- Icons are referenced by path - customize as needed
- API keys should be injected at runtime, not hardcoded
- **Always use multiple models in multi-agent systems** - see Model Selection Strategy above
- **Craft detailed system prompts** - use the system-prompt-engineer skill for guidance