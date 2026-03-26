# Node Templates Reference

This file contains all node templates for quick reference when generating multi-agent systems.

## Table of Contents
1. [Input Node](#input-node)
2. [Agent Node](#agent-node)
3. [SubAgent Node](#subagent-node)
4. [Tool Node](#tool-node)
5. [Model Node](#model-node)
6. [Node Registry Types](#node-registry-types)

---

## Input Node

**Purpose**: Entry point of the system - always the root node.

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

**Rules**:
- MUST be the first node in the array
- `referenceTo` MUST be empty `[]`
- Contains exactly one `agent` in `children`

---

## Agent Node

**Purpose**: Main supervisor/manager that orchestrates the system.

```json
{
  "node_name": "agent",
  "referenceTo": ["inputNode"],
  "config": {
    "label": "Manager Agent",
    "icon": "/icons/bot.png",
    "sub": "Tools Agent",
    "instructions": "<system prompt for the manager agent>",
    "description": "<what this agent does>"
  },
  "children": []
}
```

**Config Fields**:
| Field | Type | Description |
|-------|------|-------------|
| `label` | string | Display name of the agent |
| `icon` | string | Path to icon image |
| `sub` | string | Subtitle/type (usually "Tools Agent") |
| `instructions` | string | System prompt for the agent |
| `description` | string | Brief description of agent's purpose |

**Can Contain**: `tool`, `modelNode`, `subAgent`

---

## SubAgent Node

**Purpose**: Specialized worker agent controlled by Manager.

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

**Config Fields**:
| Field | Type | Description |
|-------|------|-------------|
| `label` | string | Display name |
| `icon` | string | Path to icon (optional) |
| `sub` | string | Subtitle (usually "worker Agent") |
| `instructions` | string | System prompt |
| `description` | string | Purpose description |
| `model` | string | Model reference (optional) |

**Can Contain**: `tool`, `modelNode`, `subAgent`

---

## Tool Node

**Purpose**: Executable capability - performs external actions.

```json
{
  "node_name": "tool",
  "referenceTo": ["<parent_node_name>"],
  "config": {
    "label": "Tool Name",
    "icon": "/icons/default.png",
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

**Icon Mapping by nodeRegistry**:
| nodeRegistry | Icon Path |
|--------------|-----------|
| `search` | `/icons/search.png` |
| `webscraper` | `/icons/crawler.png` |
| `retriever` | `/icons/retriever.png` |
| `memory` | `/icons/message.png` |
| `sheet` | `/icons/sheet.png` |
| `readSheet` | `/icons/sheet.png` |
| `pieChart` | `/icons/pie.png` |
| `lineChart` | `/icons/chart.png` |
| `vectorDB` | `/icons/db.png` |
| `embeddingModel` | `/icons/embedding.png` |
| `sendMail` | `/icons/gmail.png` |
| `calendarEvent` | `/icons/calendar.png` |
| `readFile` | `/icons/read_file.png` |
| `writeFile` | `/icons/write_file.ico` |
| `readAndUpdateFile` | `/icons/edit_file.png` |
| `imageGenerator` | `/icons/pic.png` |
| `imageReader` | `/icons/camera.png` |
| `imageEditor` | `/icons/image_editor.png` |

**Config Fields**:
| Field | Type | Description |
|-------|------|-------------|
| `label` | string | Display name |
| `icon` | string | Path to icon |
| `nodeRegistry` | string | Tool type from registry |
| `name` | string | Tool identifier |
| `config.provider` | string | Service provider (optional) |
| `config.description` | string | What the tool does |

**Rules**:
- `children` MUST be empty `[]`
- `nodeRegistry` MUST be a valid registry type

---

## Model Node

**Purpose**: Defines the AI model used by an agent/subAgent.

```json
{
  "node_name": "modelNode",
  "referenceTo": ["<parent_node_name>"],
  "config": {
    "label": "Model",
    "icon": "/icons/openai-icon.svg",
    "model": "<model_name>",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": "<provider_name>"
    }
  },
  "children": []
}
```

**Icon Mapping by Provider**:
| Provider | Icon Path |
|----------|-----------|
| OpenAI (GPT-4, GPT-3.5) | `/icons/openai-icon.svg` |
| Google Gemini | `/icons/gemini.png` |
| DeepSeek | `/icons/deepseek-icon.svg` |
| Meta Llama | `/icons/meta-icon.svg` |
| Mistral | `/icons/mistral-ai-icon.svg` |
| Qwen | `/icons/qwen-icon.svg` |
| Kimi | `/icons/kimi.svg` |
| Unknown/Fallback | `/icons/default.png` |

**Config Fields**:
| Field | Type | Description |
|-------|------|-------------|
| `label` | string | Display name |
| `icon` | string | Path to icon |
| `model` | string | Model name (e.g., "gpt", "claude", "deepseek") |
| `apiKey` | string | API key (inject at runtime) |
| `nodeRegistry` | string | Always "model" |
| `config.provider` | string | Provider name (e.g., "openai", "anthropic") |

**Rules**:
- `children` MUST be empty `[]`

---

## Node Registry Types

Valid values for `nodeRegistry` field in tool nodes:

### Search & Retrieval
| Type | Description |
|------|-------------|
| `search` | Web search capability |
| `webscraper` | Web scraping capability |
| `retriever` | Document retrieval |
| `memory` | Memory/storage operations |

### Data & Charts
| Type | Description |
|------|-------------|
| `sheet` | Spreadsheet operations |
| `readSheet` | Read spreadsheet data |
| `pieChart` | Generate pie charts |
| `lineChart` | Generate line charts |

### AI & Embeddings
| Type | Description |
|------|-------------|
| `model` | AI model |
| `vectorDB` | Vector database operations |
| `embeddingModel` | Text embeddings |

### Communication
| Type | Description |
|------|-------------|
| `sendMail` | Email sending |
| `calendarEvent` | Calendar operations |

### File Operations
| Type | Description |
|------|-------------|
| `readFile` | File reading |
| `writeFile` | File writing |
| `readAndUpdateFile` | File modification |

### Visual & Creative
| Type | Description |
|------|-------------|
| `mindMap` | Mind map generation |
| `imageGenerator` | AI image generation |
| `imageReader` | Image analysis |
| `imageEditor` | Image editing |

---

## Quick Reference: Node Hierarchy

```
inputNode (root)
└── agent (manager)
    ├── modelNode (AI model for manager)
    ├── tool (capability for manager)
    └── subAgent (worker)
        ├── modelNode (AI model for worker)
        ├── tool (capability for worker)
        └── subAgent (nested worker)
            └── ... (can nest further)
```

## Validation Checklist

- [ ] inputNode is first element
- [ ] inputNode.referenceTo is empty []
- [ ] All tool.children are empty []
- [ ] All referenceTo values point to valid parent nodes
- [ ] All nodeRegistry values are valid types
- [ ] All agents have instructions
- [ ] All tools have descriptions