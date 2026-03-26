# 📄 Tool Registry Reference

This document defines all available tools (`nodeRegistry`) and what each one does in the multi-agent system.

---

## TypeScript Definition

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

---

## 🔍 search

**Description**  
Performs real-time web search to retrieve relevant information.

**Use Cases**
- Finding up-to-date information
- Research queries
- Gathering external knowledge

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Web Search",
    "icon": "/icons/search.PNG",
    "nodeRegistry": "search",
    "name": "Web Search",
    "config": {
      "provider": "",
      "description": "Search the web for information"
    }
  },
  "children": []
}
```

---

## 🌐 webscraper

**Description**  
Extracts structured content from web pages (HTML parsing, DOM extraction).

**Use Cases**
- Scraping articles
- Extracting data from websites
- Parsing structured content

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Web Scraper",
    "icon": "/icons/webscraper.PNG",
    "nodeRegistry": "webscraper",
    "name": "Web Scraper",
    "config": {
      "provider": "",
      "description": "Extract content from web pages"
    }
  },
  "children": []
}
```

---

## 📚 retriever

**Description**  
Retrieves relevant documents from a knowledge base using similarity search.

**Use Cases**
- RAG (Retrieval-Augmented Generation)
- Document search
- Context injection for LLMs

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Document Retriever",
    "icon": "/icons/retriever.PNG",
    "nodeRegistry": "retriever",
    "name": "Retriever",
    "config": {
      "provider": "",
      "description": "Retrieve information from documents"
    }
  },
  "children": []
}
```

---

## 🧠 memory

**Description**  
Stores and retrieves conversation or system memory.

**Use Cases**
- Chat history
- Long-term context
- Personalization

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Memory",
    "icon": "/icons/memory.PNG",
    "nodeRegistry": "memory",
    "name": "Memory",
    "config": {
      "provider": "",
      "description": "Store and retrieve conversation memory"
    }
  },
  "children": []
}
```

---

## 🤖 model

**Description**  
Defines the AI model used by an agent for reasoning and generation.

**Use Cases**
- Text generation
- Decision making
- Planning and reasoning

**Example Configuration**
```json
{
  "node_name": "modelNode",
  "referenceTo": ["agent"],
  "config": {
    "label": "Model",
    "icon": "/icons/model.PNG",
    "model": "gpt-4",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": "openai"
    }
  },
  "children": []
}
```

---

## 📊 sheet

**Description**  
Creates or writes structured data into a spreadsheet.

**Use Cases**
- Logging results
- Generating reports
- Data organization

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Spreadsheet",
    "icon": "/icons/sheet.PNG",
    "nodeRegistry": "sheet",
    "name": "Sheet",
    "config": {
      "provider": "",
      "description": "Write data to spreadsheets"
    }
  },
  "children": []
}
```

---

## 📖 readSheet

**Description**  
Reads data from an existing spreadsheet.

**Use Cases**
- Data analysis
- Fetching stored records
- Feeding structured input to agents

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Read Spreadsheet",
    "icon": "/icons/readSheet.PNG",
    "nodeRegistry": "readSheet",
    "name": "Read Sheet",
    "config": {
      "provider": "",
      "description": "Read data from spreadsheets"
    }
  },
  "children": []
}
```

---

## 🥧 pieChart

**Description**  
Generates a pie chart visualization from structured data.

**Use Cases**
- Data visualization
- Reports and dashboards

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Pie Chart",
    "icon": "/icons/pieChart.PNG",
    "nodeRegistry": "pieChart",
    "name": "Pie Chart",
    "config": {
      "provider": "",
      "description": "Generate pie chart visualizations"
    }
  },
  "children": []
}
```

---

## 📈 lineChart

**Description**  
Generates a line chart for time-series or trend data.

**Use Cases**
- Trend analysis
- Performance tracking

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Line Chart",
    "icon": "/icons/lineChart.PNG",
    "nodeRegistry": "lineChart",
    "name": "Line Chart",
    "config": {
      "provider": "",
      "description": "Generate line chart visualizations"
    }
  },
  "children": []
}
```

---

## 🗄️ vectorDB

**Description**  
Stores and queries vector embeddings for semantic search.

**Use Cases**
- RAG systems
- Semantic similarity search
- Knowledge storage

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Vector Database",
    "icon": "/icons/vectorDB.PNG",
    "nodeRegistry": "vectorDB",
    "name": "VectorDB",
    "config": {
      "provider": "",
      "description": "Store and query vector embeddings"
    }
  },
  "children": []
}
```

---

## 🔢 embeddingModel

**Description**  
Converts text into vector embeddings.

**Use Cases**
- Indexing documents
- Semantic search
- Similarity comparison

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Embedding Model",
    "icon": "/icons/embeddingModel.PNG",
    "nodeRegistry": "embeddingModel",
    "name": "Embedding Model",
    "config": {
      "provider": "",
      "description": "Convert text to vector embeddings"
    }
  },
  "children": []
}
```

---

## 📧 sendMail

**Description**  
Sends emails via a configured provider.

**Use Cases**
- Notifications
- Reports delivery
- User communication

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Send Email",
    "icon": "/icons/sendMail.PNG",
    "nodeRegistry": "sendMail",
    "name": "Send Mail",
    "config": {
      "provider": "",
      "description": "Send emails to users"
    }
  },
  "children": []
}
```

---

## 📅 calendarEvent

**Description**  
Creates or manages calendar events.

**Use Cases**
- Scheduling meetings
- Reminders
- Task planning

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Calendar Event",
    "icon": "/icons/calendarEvent.PNG",
    "nodeRegistry": "calendarEvent",
    "name": "Calendar",
    "config": {
      "provider": "",
      "description": "Create and manage calendar events"
    }
  },
  "children": []
}
```

---

## 📂 readFile

**Description**  
Reads content from a file.

**Use Cases**
- Processing documents
- Loading data
- Code analysis

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Read File",
    "icon": "/icons/readFile.PNG",
    "nodeRegistry": "readFile",
    "name": "Read File",
    "config": {
      "provider": "",
      "description": "Read content from files"
    }
  },
  "children": []
}
```

---

## ✍️ writeFile

**Description**  
Creates or writes content to a file.

**Use Cases**
- Saving outputs
- Generating reports
- Code generation

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Write File",
    "icon": "/icons/writeFile.PNG",
    "nodeRegistry": "writeFile",
    "name": "Write File",
    "config": {
      "provider": "",
      "description": "Write content to files"
    }
  },
  "children": []
}
```

---

## 🔄 readAndUpdateFile

**Description**  
Reads a file, modifies its content, and writes updates back.

**Use Cases**
- Editing code
- Updating documents
- Incremental changes

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Read and Update File",
    "icon": "/icons/readAndUpdateFile.PNG",
    "nodeRegistry": "readAndUpdateFile",
    "name": "Update File",
    "config": {
      "provider": "",
      "description": "Read, modify, and update files"
    }
  },
  "children": []
}
```

---

## 🎨 imageGenerator

**Description**  
Generates images from text prompts using AI models.

**Use Cases**
- Design assets
- Visual content creation
- UI mockups

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Image Generator",
    "icon": "/icons/imageGenerator.PNG",
    "nodeRegistry": "imageGenerator",
    "name": "Image Generator",
    "config": {
      "provider": "",
      "description": "Generate images from text prompts"
    }
  },
  "children": []
}
```

---

## 🖼️ imageReader

**Description**  
Analyzes and extracts information from images.

**Use Cases**
- OCR (text extraction)
- Image understanding
- Screenshot analysis

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Image Reader",
    "icon": "/icons/imageReader.PNG",
    "nodeRegistry": "imageReader",
    "name": "Image Reader",
    "config": {
      "provider": "",
      "description": "Analyze and extract information from images"
    }
  },
  "children": []
}
```

---

## 🛠️ imageEditor

**Description**  
Modifies or enhances existing images.

**Use Cases**
- Image transformations
- Editing visuals
- Style changes

**Example Configuration**
```json
{
  "node_name": "tool",
  "referenceTo": ["agent"],
  "config": {
    "label": "Image Editor",
    "icon": "/icons/imageEditor.PNG",
    "nodeRegistry": "imageEditor",
    "name": "Image Editor",
    "config": {
      "provider": "",
      "description": "Edit and modify images"
    }
  },
  "children": []
}
```

---

# 🚀 Summary

These **19 tools** extend agent capabilities beyond text generation, enabling:
- **External data access**: search, webscraper, retriever
- **File operations**: readFile, writeFile, readAndUpdateFile
- **Visualization**: pieChart, lineChart
- **Communication**: sendMail, calendarEvent
- **Data management**: sheet, readSheet, vectorDB, embeddingModel, memory
- **Multimodal processing**: imageGenerator, imageReader, imageEditor
- **AI reasoning**: model

---

# ⚠️ Validation Rules

1. **ONLY these 19 tool types are valid** for the `nodeRegistry` field
2. **DO NOT** create tools with `nodeRegistry` values not in this list
3. **WARN the user** if they request a tool type that doesn't exist
4. **Suggest alternatives** if a requested capability isn't available

## Common Mistakes to Avoid

| Invalid Type | Why It's Wrong | Valid Alternative |
|--------------|----------------|-------------------|
| `mindMap` | Not in NodeRegistryType | `imageGenerator` (can create visual diagrams) |
| `weatherAPI` | Not in NodeRegistryType | `search` + `webscraper` (fetch weather data) |
| `translate` | Not in NodeRegistryType | `model` (use a translation-capable model) |
| `calculator` | Not in NodeRegistryType | `model` (use a model with math capabilities) |