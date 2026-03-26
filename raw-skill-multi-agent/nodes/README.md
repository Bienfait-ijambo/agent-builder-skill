# 📄 Multi-Agent Node System Specification

## 🧠 Overview

This system defines a **Supervisor Multi-Agent Architecture** using a tree + graph structure.

Each node:
- Has a **type** (`node_name`)
- Can reference other nodes (`referenceTo`)
- Contains configuration (`config`)
- Can have children (`children`)

---

## 🧩 Core Rules

- The system MUST start with an `inputNode`
- `inputNode` is always the root
- `referenceTo` defines connections between nodes
- `children` defines hierarchy (tree structure)
- `tool` nodes MUST have empty `children`
- `subAgent` can contain:
  - `tool`
  - `modelNode`
  - other `subAgent`
- `nodeRegistry` MUST match a valid registry type

---

## 🔗 Node Relationship Model

- `inputNode → agent`
- `agent → tool`
- `agent → modelNode`
- `agent → subAgent`
- `subAgent → tool`
- `subAgent → modelNode`
- `subAgent → subAgent`

---

## 🧩 Node Definitions

---

### 1️⃣ Input Node

```json
{
  "node_name": "inputNode",
  "referenceTo": [],
  "config": {
    "label": "When chat message received",
    "icon": "/icons/input.PNG",
    "ui": {}
  },
  "children": []
}
```

**Description**
- Entry point of the system
- Receives user input

**Responsibilities**
- Trigger workflow execution
- Pass input to Manager Agent

---

### 2️⃣ Agent Node (Manager)

```json
{
  "node_name": "agent",
  "referenceTo": [],
  "config": {
    "label": "Manager Agent",
    "icon": "",
    "sub": "Tools Agent",
    "instructions": "",
    "description": ""
  },
  "children": []
}
```

**Description**
- Main supervisor agent
- Orchestrates the entire system

**Responsibilities**
- Delegates tasks
- Uses tools
- Calls subAgents
- Controls execution flow

---

### 3️⃣ Tool Node

```json
{
  "node_name": "tool",
  "referenceTo": [],
  "config": {
    "label": "Web Search",
    "icon": "/icons/search.PNG",
    "nodeRegistry": "search",
    "name": "Web Search",
    "config": {
      "provider": "",
      "description": ""
    }
  },
  "children": []
}
```

**Description**
- Represents an executable capability

**Responsibilities**
- Perform external actions
- Fetch or process data

**Rules**
- `children` MUST be empty
- Must use valid `nodeRegistry`

---

### 4️⃣ Model Node

```json
{
  "node_name": "modelNode",
  "referenceTo": [],
  "config": {
    "label": "",
    "icon": "/icons/deepseek-icon.svg",
    "model": "",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": ""
    }
  },
  "children": []
}
```

**Description**
- Defines the AI model used by an agent

**Responsibilities**
- Attach intelligence to agent/subAgent
- Control reasoning vs speed

---

### 5️⃣ SubAgent Node

```json
{
  "node_name": "subAgent",
  "referenceTo": [],
  "config": {
    "label": "Worker Agent",
    "icon": "",
    "sub": "worker Agent",
    "instructions": "",
    "model": ""
  },
  "children": []
}
```

**Description**
- Specialized agent controlled by Manager

**Responsibilities**
- Execute specific tasks
- Can use tools and models
- Can contain nested subAgents

---

## 🧾 Node Registry Types

```ts
type NodeRegistryType =
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
  | "mindMap"
  | "imageGenerator"
  | "imageReader"
  | "imageEditor"
```

---

## 🧪 Example Patterns

### ✅ Manager with Tools (No Model)

- Manager uses tools only
- No reasoning model attached

### ✅ Manager with Model

- Manager includes a `modelNode`
- Enables reasoning

### ✅ Manager + SubAgent

- Manager delegates to subAgent
- subAgent has its own tools + model

---

## ⚡ Key Design Principles

- Keep agents **modular**
- Use **subAgents for specialization**
- Attach **models only where needed**
- Use `referenceTo` for graph flexibility
- Use `children` for hierarchy

---

## 🚀 Goal

This structure enables:
- Visual workflow builders
- Recursive agent systems
- Supervisor-based orchestration
- Scalable AI architectures