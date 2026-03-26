# Icon Registry Reference

This document defines all available icons in the `icons/` folder and their correct usage for each node type and tool.

---

## 📁 Icon Folder Location

All icons are located at: `skills/multi-agent-builder/icons/`

When referencing icons in JSON config, use the path format: `/icons/<filename>`

---

## 🎯 MANDATORY Icon Rules

1. **Every node MUST have an `icon` field** in its `config` object
2. **Icon paths MUST reference files that exist** in the icons folder
3. **Use the correct icon for the node type** (see mappings below)
4. **Model nodes should use provider-specific icons** when available

---

## 📋 Icon Mapping by Node Type

### 1. Input Node (`inputNode`)

| Icon File | Path | Use Case |
|-----------|------|----------|
| `input.png` | `/icons/input.png` | **DEFAULT** - Entry point for chat messages |

**Example:**
```json
{
  "node_name": "inputNode",
  "config": {
    "label": "When chat message received",
    "icon": "/icons/input.png"
  }
}
```

---

### 2. Agent Node (`agent`)

| Icon File | Path | Use Case |
|-----------|------|----------|
| `bot.png` | `/icons/bot.png` | **DEFAULT** - General agent/bot |
| `brain.png` | `/icons/brain.png` | Intelligent/reasoning agent |
| `chatbot.png` | `/icons/chatbot.png` | Conversational agent |

**Example:**
```json
{
  "node_name": "agent",
  "config": {
    "label": "Manager Agent",
    "icon": "/icons/bot.png",
    "sub": "Tools Agent"
  }
}
```

---

### 3. SubAgent Node (`subAgent`)

| Icon File | Path | Use Case |
|-----------|------|----------|
| `bot.png` | `/icons/bot.png` | **DEFAULT** - Worker agent |
| `chatbot.png` | `/icons/chatbot.png` | Conversational worker |
| `default.png` | `/icons/default.png` | Fallback for any type |

**Example:**
```json
{
  "node_name": "subAgent",
  "config": {
    "label": "Research Worker",
    "icon": "/icons/bot.png",
    "sub": "worker Agent"
  }
}
```

---

### 4. Model Node (`modelNode`)

Use provider-specific icons based on the model:

| Icon File | Path | Provider/Model |
|-----------|------|----------------|
| `openai-icon.svg` | `/icons/openai-icon.svg` | **OpenAI** (GPT-4, GPT-3.5, etc.) |
| `gemini.png` | `/icons/gemini.png` | **Google Gemini** |
| `deepseek-icon.svg` | `/icons/deepseek-icon.svg` | **DeepSeek** |
| `meta-icon.svg` | `/icons/meta-icon.svg` | **Meta Llama** |
| `mistral-ai-icon.svg` | `/icons/mistral-ai-icon.svg` | **Mistral AI** |
| `qwen-icon.svg` | `/icons/qwen-icon.svg` | **Qwen (Alibaba)** |
| `kimi.svg` | `/icons/kimi.svg` | **Kimi** |
| `default.png` | `/icons/default.png` | Unknown/fallback provider |

**Example:**
```json
{
  "node_name": "modelNode",
  "config": {
    "label": "GPT-4 Model",
    "icon": "/icons/openai-icon.svg",
    "model": "gpt-4",
    "nodeRegistry": "model",
    "config": {
      "provider": "openai"
    }
  }
}
```

---

## 🔧 Icon Mapping by Tool (nodeRegistry)

### Search Tools

| nodeRegistry | Icon File | Path |
|--------------|-----------|------|
| `search` | `search.png` | `/icons/search.png` |
| `search` (alternative) | `globe.svg` | `/icons/globe.svg` |
| `search` (alternative) | `google.png` | `/icons/google.png` |

---

### Web Scraping

| nodeRegistry | Icon File | Path |
|--------------|-----------|------|
| `webscraper` | `crawler.png` | `/icons/crawler.png` |

---

### Document Retrieval

| nodeRegistry | Icon File | Path |
|--------------|-----------|------|
| `retriever` | `retriever.png` | `/icons/retriever.png` |

---

### Memory

| nodeRegistry | Icon File | Path |
|--------------|-----------|------|
| `memory` | `message.png` | `/icons/message.png` |

---

### Spreadsheet Tools

| nodeRegistry | Icon File | Path |
|--------------|-----------|------|
| `sheet` | `sheet.png` | `/icons/sheet.png` |
| `readSheet` | `sheet.png` | `/icons/sheet.png` |

---

### Chart Tools

| nodeRegistry | Icon File | Path |
|--------------|-----------|------|
| `pieChart` | `pie.png` | `/icons/pie.png` |
| `lineChart` | `chart.png` | `/icons/chart.png` |

---

### Vector Database

| nodeRegistry | Icon File | Path |
|--------------|-----------|------|
| `vectorDB` | `db.png` | `/icons/db.png` |
| `vectorDB` (alternative) | `pinecone.png` | `/icons/pinecone.png` |

---

### Embedding Model

| nodeRegistry | Icon File | Path |
|--------------|-----------|------|
| `embeddingModel` | `embedding.png` | `/icons/embedding.png` |

---

### Communication Tools

| nodeRegistry | Icon File | Path |
|--------------|-----------|------|
| `sendMail` | `gmail.png` | `/icons/gmail.png` |
| `sendMail` (alternative) | `slack.png` | `/icons/slack.png` |
| `calendarEvent` | `calendar.png` | `/icons/calendar.png` |

---

### File Operations

| nodeRegistry | Icon File | Path |
|--------------|-----------|------|
| `readFile` | `read_file.png` | `/icons/read_file.png` |
| `readFile` (alternative) | `file.svg` | `/icons/file.svg` |
| `readFile` (PDF) | `pdf.png` | `/icons/pdf.png` |
| `writeFile` | `write_file.ico` | `/icons/write_file.ico` |
| `readAndUpdateFile` | `edit_file.png` | `/icons/edit_file.png` |

---

### Image Tools

| nodeRegistry | Icon File | Path |
|--------------|-----------|------|
| `imageGenerator` | `pic.png` | `/icons/pic.png` |
| `imageReader` | `camera.png` | `/icons/camera.png` |
| `imageEditor` | `image_editor.png` | `/icons/image_editor.png` |

---

## 📊 Quick Reference Table

| Node Type / Tool | Recommended Icon | Path |
|------------------|------------------|------|
| `inputNode` | input.png | `/icons/input.png` |
| `agent` | bot.png | `/icons/bot.png` |
| `subAgent` | bot.png | `/icons/bot.png` |
| `modelNode` (OpenAI) | openai-icon.svg | `/icons/openai-icon.svg` |
| `modelNode` (Gemini) | gemini.png | `/icons/gemini.png` |
| `modelNode` (DeepSeek) | deepseek-icon.svg | `/icons/deepseek-icon.svg` |
| `search` | search.png | `/icons/search.png` |
| `webscraper` | crawler.png | `/icons/crawler.png` |
| `retriever` | retriever.png | `/icons/retriever.png` |
| `memory` | message.png | `/icons/message.png` |
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

---

## ⚠️ Common Mistakes to Avoid

### ❌ WRONG: Using non-existent icon
```json
{
  "config": {
    "icon": "/icons/weather.PNG"  // This file doesn't exist!
  }
}
```

### ❌ WRONG: Empty icon field
```json
{
  "config": {
    "icon": ""  // Empty icon - will show broken image
  }
}
```

### ❌ WRONG: Wrong extension case
```json
{
  "config": {
    "icon": "/icons/search.PNG"  // File is actually search.png (lowercase)
  }
}
```

### ✅ CORRECT: Using valid icon with correct case
```json
{
  "config": {
    "icon": "/icons/search.png"  // Correct path and case
  }
}
```

---

## 📁 Complete Icon Inventory

All 41 available icons in the `icons/` folder:

| Filename | Extension | Recommended Use |
|----------|-----------|-----------------|
| bot.png | .png | Agent/SubAgent (DEFAULT) |
| brain.png | .png | Agent (reasoning) |
| calendar.png | .png | calendarEvent tool |
| camera.png | .png | imageReader tool |
| chart.png | .png | lineChart tool |
| chatbot.png | .png | Agent/SubAgent (conversational) |
| crawler.png | .png | webscraper tool |
| db.png | .png | vectorDB tool |
| deepseek-icon.svg | .svg | DeepSeek model |
| default.png | .png | Fallback for any node |
| drive.png | .png | Google Drive integration |
| edit_file.png | .png | readAndUpdateFile tool |
| embedding.png | .png | embeddingModel tool |
| file.svg | .svg | readFile tool (alternative) |
| gemini.png | .png | Gemini model |
| globe.svg | .svg | search tool (alternative) |
| gmail.png | .png | sendMail tool |
| google.png | .png | search tool (alternative) |
| image_editor.png | .png | imageEditor tool |
| input.png | .png | inputNode (DEFAULT) |
| kimi.svg | .svg | Kimi model |
| message.png | .png | memory tool |
| meta-icon.svg | .svg | Meta Llama model |
| mistral-ai-icon.svg | .svg | Mistral model |
| notion.png | .png | Notion integration |
| openai-icon.svg | .svg | OpenAI model |
| output.png | .png | Output node |
| pdf-1512.svg | .svg | PDF document |
| pdf.png | .png | readFile tool (PDF files) |
| pic.png | .png | imageGenerator tool |
| pie.png | .png | pieChart tool |
| pinecone.png | .png | vectorDB tool (Pinecone) |
| pinecone.svg | .svg | vectorDB tool (Pinecone) |
| qwen-icon.svg | .svg | Qwen model |
| react.svg | .svg | React/React Flow |
| read_file.png | .png | readFile tool |
| retriever.png | .png | retriever tool |
| search.png | .png | search tool (DEFAULT) |
| sheet.png | .png | sheet/readSheet tool |
| slack.png | .png | sendMail tool (alternative) |
| window.svg | .svg | UI/Desktop |
| write_file.ico | .ico | writeFile tool |

---

## 🔄 Icon Selection Algorithm

When building an agent, follow this process:

1. **Identify the node type** (inputNode, agent, subAgent, tool, modelNode)
2. **For tools**: Match the `nodeRegistry` value to the tool icon table
3. **For models**: Match the provider to the model icon table
4. **Verify the icon exists** in the icons folder
5. **Use the correct file extension case** (most are lowercase `.png`)

---

## 📝 Example: Complete Agent with Correct Icons

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
          "label": "Research Agent",
          "icon": "/icons/bot.png",
          "sub": "Tools Agent",
          "instructions": "You are a research assistant.",
          "description": "Helps users research topics"
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
                "description": "Search the web"
              }
            },
            "children": []
          },
          {
            "node_name": "modelNode",
            "referenceTo": ["agent"],
            "config": {
              "label": "GPT-4",
              "icon": "/icons/openai-icon.svg",
              "model": "gpt-4",
              "apiKey": "",
              "nodeRegistry": "model",
              "config": {
                "provider": "openai"
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