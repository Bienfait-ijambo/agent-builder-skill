# 📄 Multi-Agent Supervisor System — README

## 🧠 Overview

This project defines a **Supervisor-based Multi-Agent System** using a structured JSON format.

The system is built using:
- A **tree structure** (`children`) for execution hierarchy
- A **graph structure** (`referenceTo`) for node relationships
-  instructions, property represent the system prompt,
- description : describe what the agent or subagent does. for tools what the tools does
---

## 🧩 Core Concepts

### Node Types
- `inputNode` → entry point
- `agent` → manager (supervisor)
- `subAgent` → specialized worker
- `tool` → executable capability
- `modelNode` → AI model attachment

---

## ⚙️ Rules

- System MUST start with `inputNode`
- `inputNode` is root
- `tool.children` MUST be empty
- `subAgent.children` can contain:
  - tool
  - modelNode
  - subAgent
- `referenceTo` defines node links
- `nodeRegistry` must be valid

---

## 🧾 Node Registry Types

```ts
type NodeRegistryType =
  | "search"
  | "webscraper"
  | "retriever"
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
  | "mindMap"
  | "imageGenerator"
  | "imageReader"
  | "imageEditor"
```

---

# 🧪 Examples

---

## ✅ Example 1 — Manager with Tools (No Model)

```json
[
  {
    "node_name": "inputNode",
    "referenceTo": [],
    "config": {
      "label": "Input",
      "icon": "/icons/input.PNG",
      "ui": {}
    },
    "children": [
      {
        "node_name": "agent",
        "referenceTo": ["inputNode"],
        "config": {
          "label": "Manager Agent",
          "sub": "Tools Agent",
          "instructions": "dkdkkdkddkdkkdkdkdk",
          "description": "dkdkkdk",
        },
        "children": [
          {
            "node_name": "tool",
            "referenceTo": ["agent"],
            "config": {
              "label": "Search",
              "nodeRegistry": "search",
              "name": "Search",
              "config": {
                "provider":"",
                "description":"dkdkdk"
              }
            },
            "children": []
          },
          {
            "node_name": "modelNode",
            "referenceTo": ["agent"],
            "config": {
              "label": "Search",
              "nodeRegistry": "search",
              "name": "Search",
              "config": {
                "provider":"",
                "description":""
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

---

## ✅ Example 2 — Manager with Model

```json
[
  {
    "node_name": "inputNode",
    "referenceTo": [],
    "config": { "label": "Input", "icon": "", "ui": {} },
    "children": [
      {
        "node_name": "agent",
        "referenceTo": ["inputNode"],
        "config": { "label": "Manager Agent" },
        "children": [
          {
            "node_name": "modelNode",
            "referenceTo": ["agent"],
            "config": {
              "nodeRegistry": "model",
              "model": "gpt",
              "config": { 
                "provider": "openai" ,
               "instructions": "",
          "description": "",}
            },
            "children": []
          }
        ]
      }
    ]
  }
]
```

---

## ✅ Example 3 — Manager + Multiple Tools

```json
[
  {
    "node_name": "inputNode",
    "referenceTo": [],
    "config": { "label": "Input", "icon": "", "ui": {} },
    "children": [
      {
        "node_name": "agent",
        "referenceTo": ["inputNode"],
        "config": { "label": "Manager Agent" },
        "children": [
          {
            "node_name": "tool",
            "referenceTo": ["agent"],
            "config": { "nodeRegistry": "search", "name": "Search","description":"" },
            "children": []
          },
          {
            "node_name": "tool",
            "referenceTo": ["agent"],
            "config": { "nodeRegistry": "webscraper", "name": "Scraper","description":"" },
            "children": []
          }
        ]
      }
    ]
  }
]
```

---

## ✅ Example 4 — Manager + SubAgent

```json
[
  {
    "node_name": "inputNode",
    "referenceTo": [],
    "config": { "label": "Input", "ui": {} },
    "children": [
      {
        "node_name": "agent",
        "referenceTo": ["inputNode"],
        "config": { "label": "Manager" },
        "children": [
          {
            "node_name": "subAgent",
            "referenceTo": ["agent"],
            "config": { "label": "Worker Agent", "instructions": "",
          "description": "", },
            "children": []
          }
        ]
      }
    ]
  }
]
```

---

## ✅ Example 5 — SubAgent with Tools + Model

```json
[
  {
    "node_name": "inputNode",
    "referenceTo": [],
    "config": { "label": "Input", "ui": {} },
    "children": [
      {
        "node_name": "agent",
        "referenceTo": ["inputNode"],
        "config": { "label": "Manager" },
        "children": [
          {
            "node_name": "subAgent",
            "referenceTo": ["agent"],
            "config": { "label": "Research Agent", "instructions": "",
          "description": "" },
            "children": [
              {
                "node_name": "modelNode",
                "referenceTo": ["subAgent"],
                "config": {
                  "nodeRegistry": "model",
                  "model": "deepseek"
                },
                "children": []
              },
              {
                "node_name": "tool",
                "referenceTo": ["subAgent"],
                "config": {
                  "nodeRegistry": "search",
                  "name": "Search",
                   "instructions": "",
          "description": "",
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

---

## ✅ Example 6 — Nested SubAgents

```json
[
  {
    "node_name": "inputNode",
    "referenceTo": [],
    "config": { "label": "Input", "ui": {} },
    "children": [
      {
        "node_name": "agent",
        "referenceTo": ["inputNode"],
        "config": { "label": "Manager", "instructions": "",
          "description": "", },
        "children": [
          {
            "node_name": "subAgent",
            "referenceTo": ["agent"],
            "config": { "label": "Parent Agent", "instructions": "",
          "description": "" },
            "children": [
              {
                "node_name": "subAgent",
                "referenceTo": ["subAgent"],
                "config": { "label": "Child Agent", "instructions": "",
          "description": "", },
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

---

## ✅ Example 7 — Full System (Manager + Tools + Model + SubAgent)

```json
[
  {
    "node_name": "inputNode",
    "referenceTo": [],
    "config": { "label": "Input", "ui": {} },
    "children": [
      {
        "node_name": "agent",
        "referenceTo": ["inputNode"],
        "config": { "label": "Manager Agent" , "instructions": "",
          "description": ""},
        "children": [
          {
            "node_name": "tool",
            "referenceTo": ["agent"],
            "config": { 
                "nodeRegistry": "search",
                 "name": "Search" ,
          "description": ""},
            "children": []
          },
          {
            "node_name": "modelNode",
            "referenceTo": ["agent"],
            "config": {
              "nodeRegistry": "model",
              "model": "gpt"
            },
            "children": []
          },
          {
            "node_name": "subAgent",
            "referenceTo": ["agent"],
            "config": { "label": "Analyzer Agent" , "instructions": "",
          "description": ""},
            "children": [
              {
                "node_name": "tool",
                "referenceTo": ["subAgent"],
                "config": {
                  "nodeRegistry": "retriever",
                  "name": "Retriever",
                  "description"
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

---

# 🚀 Summary

This structure enables:
- Recursive agent systems
- Supervisor orchestration
- Tool-based execution
- Multi-model architectures
- Visual workflow builders
