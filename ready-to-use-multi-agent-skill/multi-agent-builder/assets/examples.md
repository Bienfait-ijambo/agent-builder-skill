# Multi-Agent System Examples

This file contains complete examples of multi-agent system JSON structures.

---

## Example 1: Simple Agent with Tool

A basic agent with a single search tool.

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
          "instructions": "You are a helpful assistant that can search the web.",
          "description": "An agent with web search capability"
        },
        "children": [
          {
            "node_name": "tool",
            "referenceTo": ["agent"],
            "config": {
              "label": "Search",
              "icon": "/icons/search.PNG",
              "nodeRegistry": "search",
              "name": "Search",
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

---

## Example 2: Agent with Model

A manager agent with an attached AI model.

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
          "instructions": "You are an intelligent assistant powered by GPT.",
          "description": "A manager agent using GPT model"
        },
        "children": [
          {
            "node_name": "modelNode",
            "referenceTo": ["agent"],
            "config": {
              "label": "Model",
              "icon": "/icons/model.PNG",
              "nodeRegistry": "model",
              "model": "gpt",
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

---

## Example 3: Manager with Multiple Tools

A manager agent with multiple capabilities.

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
          "instructions": "You are a versatile assistant that can search the web and scrape content.",
          "description": "A multi-tool agent"
        },
        "children": [
          {
            "node_name": "tool",
            "referenceTo": ["agent"],
            "config": {
              "label": "Search",
              "icon": "/icons/search.PNG",
              "nodeRegistry": "search",
              "name": "Search",
              "config": {
                "provider": "",
                "description": "Search the web"
              }
            },
            "children": []
          },
          {
            "node_name": "tool",
            "referenceTo": ["agent"],
            "config": {
              "label": "Scraper",
              "icon": "/icons/webscraper.PNG",
              "nodeRegistry": "webscraper",
              "name": "Scraper",
              "config": {
                "provider": "",
                "description": "Scrape web content"
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

## Example 4: Manager with SubAgent

A supervisor pattern with a worker agent.

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
          "label": "Manager",
          "instructions": "You are a manager that delegates tasks to the worker agent.",
          "description": "Supervisor agent"
        },
        "children": [
          {
            "node_name": "subAgent",
            "referenceTo": ["agent"],
            "config": {
              "label": "Worker Agent",
              "instructions": "You are a worker that executes tasks assigned by the manager.",
              "description": "Worker agent for task execution"
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

## Example 5: SubAgent with Tools + Model

A specialized worker with its own tools and model.

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
          "label": "Manager",
          "instructions": "You coordinate research tasks.",
          "description": "Research coordinator"
        },
        "children": [
          {
            "node_name": "subAgent",
            "referenceTo": ["agent"],
            "config": {
              "label": "Research Agent",
              "instructions": "You conduct research using web search.",
              "description": "Specialized research agent"
            },
            "children": [
              {
                "node_name": "modelNode",
                "referenceTo": ["subAgent"],
                "config": {
                  "label": "Model",
                  "icon": "/icons/model.PNG",
                  "nodeRegistry": "model",
                  "model": "deepseek"
                },
                "children": []
              },
              {
                "node_name": "tool",
                "referenceTo": ["subAgent"],
                "config": {
                  "label": "Search",
                  "icon": "/icons/search.PNG",
                  "nodeRegistry": "search",
                  "name": "Search",
                  "config": {
                    "description": "Search for research data"
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

---

## Example 6: Nested SubAgents

A hierarchical structure with nested workers.

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
          "label": "Manager",
          "instructions": "You are the top-level manager.",
          "description": "Top-level supervisor"
        },
        "children": [
          {
            "node_name": "subAgent",
            "referenceTo": ["agent"],
            "config": {
              "label": "Parent Agent",
              "instructions": "You manage a team of child agents.",
              "description": "Middle-level manager"
            },
            "children": [
              {
                "node_name": "subAgent",
                "referenceTo": ["subAgent"],
                "config": {
                  "label": "Child Agent",
                  "instructions": "You execute specific tasks.",
                  "description": "Leaf-level worker"
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

## Example 7: Full System (Manager + Tools + Model + SubAgent)

A complete multi-agent system with all components.

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
          "instructions": "You are the main supervisor. Coordinate between tools and sub-agents to accomplish user tasks.",
          "description": "Main supervisor agent"
        },
        "children": [
          {
            "node_name": "tool",
            "referenceTo": ["agent"],
            "config": {
              "label": "Search",
              "icon": "/icons/search.PNG",
              "nodeRegistry": "search",
              "name": "Search",
              "config": {
                "description": "Web search capability"
              }
            },
            "children": []
          },
          {
            "node_name": "modelNode",
            "referenceTo": ["agent"],
            "config": {
              "label": "Model",
              "icon": "/icons/model.PNG",
              "nodeRegistry": "model",
              "model": "gpt"
            },
            "children": []
          },
          {
            "node_name": "subAgent",
            "referenceTo": ["agent"],
            "config": {
              "label": "Analyzer Agent",
              "instructions": "You analyze data and provide insights.",
              "description": "Data analysis specialist"
            },
            "children": [
              {
                "node_name": "tool",
                "referenceTo": ["subAgent"],
                "config": {
                  "label": "Retriever",
                  "icon": "/icons/retriever.PNG",
                  "nodeRegistry": "retriever",
                  "name": "Retriever",
                  "config": {
                    "description": "Retrieve data for analysis"
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

---

## Example 8: Content Creation Team

A team for content creation with specialized roles.

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
          "label": "Content Manager",
          "instructions": "You are a content manager. Coordinate between the researcher, writer, and image generator to create comprehensive content.",
          "description": "Manages content creation workflow"
        },
        "children": [
          {
            "node_name": "subAgent",
            "referenceTo": ["agent"],
            "config": {
              "label": "Researcher",
              "instructions": "You research topics thoroughly using web search. Provide detailed findings.",
              "description": "Research specialist"
            },
            "children": [
              {
                "node_name": "tool",
                "referenceTo": ["subAgent"],
                "config": {
                  "label": "Web Search",
                  "icon": "/icons/search.PNG",
                  "nodeRegistry": "search",
                  "name": "Web Search",
                  "config": {
                    "description": "Search for information"
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
              "label": "Writer",
              "instructions": "You write engaging content based on research. Create clear and compelling text.",
              "description": "Content writing specialist"
            },
            "children": [
              {
                "node_name": "tool",
                "referenceTo": ["subAgent"],
                "config": {
                  "label": "File Writer",
                  "icon": "/icons/writeFile.PNG",
                  "nodeRegistry": "writeFile",
                  "name": "File Writer",
                  "config": {
                    "description": "Write content to files"
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
              "label": "Image Creator",
              "instructions": "You generate relevant images for content using AI image generation.",
              "description": "Image generation specialist"
            },
            "children": [
              {
                "node_name": "tool",
                "referenceTo": ["subAgent"],
                "config": {
                  "label": "Image Gen",
                  "icon": "/icons/imageGenerator.PNG",
                  "nodeRegistry": "imageGenerator",
                  "name": "Image Gen",
                  "config": {
                    "description": "Generate AI images"
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