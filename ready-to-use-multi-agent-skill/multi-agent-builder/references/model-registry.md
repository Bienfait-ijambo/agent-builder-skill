# 🤖 Model Registry Reference

This document defines all available models for the multi-agent system. When adding a model node to an agent, you MUST use one of the models listed in this registry.

---

## ⚠️ Model Clarification Protocol (MANDATORY)

**When building an agent that includes a model node, you MUST ask the user which specific model they want to use.**

### Why This Matters

1. **Performance Differences** - Each model has different capabilities and performance characteristics
2. **Cost Considerations** - Different models have different pricing structures
3. **Task Suitability** - Some models are better suited for specific tasks (reasoning, fast responses, etc.)
4. **User Preference** - Users may have preferences based on their experience or requirements

### How to Ask for Model Selection

1. **Present available options** organized by provider
2. **Show model name and provider** clearly
3. **Do NOT assume a default model**
4. **Wait for explicit user choice** before generating JSON

### Example Clarification

```
User: "Create a research agent with a model"

Agent: I'd be happy to create a research agent! Before I proceed, I need to know which AI model you'd like to use.

**Available Models:**

**Fireworks Provider:**
- deepseek-v3p1 - DeepSeek V3.1 (high performance)
- kimi-k2-instruct-0905 - Kimi K2 Instruct
- kimi-k2-thinking - Kimi K2 Thinking (reasoning model)
- deepseek-v3p1-terminus - DeepSeek V3.1 Terminus
- gpt-oss-120b - GPT-OSS 120B (OpenAI compatible)

**Cerebras Provider:**
- llama-3.3-70b - Meta Llama 3.3 70B
- gpt-oss-120b - GPT-OSS 120B (OpenAI compatible)
- qwen-3-32b - Qwen 3 32B

**Google Provider:**
- gemini-2.5-pro - Google Gemini 2.5 Pro (most capable)
- gemini-2.5-flash - Google Gemini 2.5 Flash (fast)

Which model would you like to use?
```

---

## Complete Model Registry

### CHAT_FIREWORKS Provider

Models hosted on Fireworks AI infrastructure.

| Model Name | Model ID | Icon | Best For |
|------------|----------|------|----------|
| DeepSeek V3.1 | `accounts/fireworks/models/deepseek-v3p1` | `/icons/deepseek-icon.svg` | Complex reasoning, coding, analysis |
| Kimi K2 Instruct | `accounts/fireworks/models/kimi-k2-instruct-0905` | `/icons/kimi.svg` | General purpose, instruction following |
| Kimi K2 Thinking | `accounts/fireworks/models/kimi-k2-thinking` | `/icons/kimi.svg` | Deep reasoning, step-by-step thinking |
| DeepSeek V3.1 Terminus | `accounts/fireworks/models/deepseek-v3p1-terminus` | `/icons/deepseek-icon.svg` | Extended context, complex tasks |
| GPT-OSS 120B | `accounts/fireworks/models/gpt-oss-120b` | `/icons/openai-icon.svg` | Large-scale tasks, OpenAI compatibility |

### CHAT_CEREBRAS Provider

Models hosted on Cerebras infrastructure (fast inference).

| Model Name | Model ID | Icon | Best For |
|------------|----------|------|----------|
| Llama 3.3 70B | `llama-3.3-70b` | `/icons/meta-icon.svg` | Fast inference, general purpose |
| GPT-OSS 120B | `gpt-oss-120b` | `/icons/openai-icon.svg` | Large-scale tasks, fast responses |
| Qwen 3 32B | `qwen-3-32b` | `/icons/qwen-icon.svg` | Multilingual, coding, reasoning |

### GOOGLE Provider

Google's Gemini models.

| Model Name | Model ID | Icon | Best For |
|------------|----------|------|----------|
| Gemini 2.5 Pro | `gemini-2.5-pro` | `/icons/gemini.png` | Complex tasks, deep reasoning, multimodal |
| Gemini 2.5 Flash | `gemini-2.5-flash` | `/icons/gemini.png` | Fast responses, high throughput |

---

## Model Node JSON Structure

### Required Fields

```json
{
  "node_name": "modelNode",
  "referenceTo": ["<parent_node_name>"],
  "config": {
    "label": "<Display Name>",
    "icon": "<icon_path>",
    "model": "<exact_model_id>",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": "<provider_name>"
    }
  },
  "children": []
}
```

### Field Descriptions

| Field | Description | Required |
|-------|-------------|----------|
| `node_name` | Always `"modelNode"` | ✅ |
| `referenceTo` | Array with parent node name | ✅ |
| `label` | Human-readable model name | ✅ |
| `icon` | Path to model icon (from registry) | ✅ |
| `model` | Exact model ID (from registry) | ✅ |
| `apiKey` | API key (leave empty for runtime injection) | ✅ |
| `nodeRegistry` | Always `"model"` | ✅ |
| `config.provider` | Provider name (CHAT_FIREWORKS, CHAT_CEREBRAS, GOOGLE) | ✅ |
| `children` | Always empty array `[]` | ✅ |

---

## Complete JSON Examples

### DeepSeek V3.1 (Fireworks)

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

### Kimi K2 Thinking (Fireworks)

```json
{
  "node_name": "modelNode",
  "referenceTo": ["agent"],
  "config": {
    "label": "Kimi K2 Thinking",
    "icon": "/icons/kimi.svg",
    "model": "accounts/fireworks/models/kimi-k2-thinking",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": "CHAT_FIREWORKS"
    }
  },
  "children": []
}
```

### Llama 3.3 70B (Cerebras)

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

### Qwen 3 32B (Cerebras)

```json
{
  "node_name": "modelNode",
  "referenceTo": ["agent"],
  "config": {
    "label": "Qwen 3 32B",
    "icon": "/icons/qwen-icon.svg",
    "model": "qwen-3-32b",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": "CHAT_CEREBRAS"
    }
  },
  "children": []
}
```

### Gemini 2.5 Pro (Google)

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

### Gemini 2.5 Flash (Google)

```json
{
  "node_name": "modelNode",
  "referenceTo": ["agent"],
  "config": {
    "label": "Gemini 2.5 Flash",
    "icon": "/icons/gemini.png",
    "model": "gemini-2.5-flash",
    "apiKey": "",
    "nodeRegistry": "model",
    "config": {
      "provider": "GOOGLE"
    }
  },
  "children": []
}
```

---

## Model Selection Guide

### By Task Type

| Task Type | Recommended Models | Why |
|-----------|-------------------|-----|
| **Complex Reasoning** | DeepSeek V3.1, Kimi K2 Thinking, Gemini 2.5 Pro | Strong reasoning capabilities |
| **Fast Responses** | Gemini 2.5 Flash, Llama 3.3 70B (Cerebras) | Optimized for speed |
| **Coding Tasks** | DeepSeek V3.1, Qwen 3 32B | Excellent code generation |
| **Multilingual** | Qwen 3 32B, Gemini 2.5 Pro | Strong multilingual support |
| **Large Context** | DeepSeek V3.1 Terminus, Gemini 2.5 Pro | Extended context windows |
| **Cost-Effective** | Gemini 2.5 Flash, Qwen 3 32B | Good balance of performance and cost |

### By Provider

| Provider | Strengths | Best For |
|----------|-----------|----------|
| **CHAT_FIREWORKS** | DeepSeek models, Kimi models | Complex reasoning, thinking tasks |
| **CHAT_CEREBRAS** | Fast inference, Llama models | High-throughput, real-time applications |
| **GOOGLE** | Multimodal, Gemini ecosystem | Vision tasks, Google integration |

---

## Validation Rules

### ✅ Valid Model Node

1. Model ID matches exactly from registry (case-sensitive)
2. Icon matches the model's provider
3. Provider is correctly specified
4. All required fields present

### ❌ Invalid Model Node

1. Model ID not in registry
2. Wrong icon for provider
3. Missing required fields
4. Assumed default without asking user

### Common Mistakes

```json
// ❌ WRONG: Model ID not in registry
"model": "gpt-4-turbo"

// ❌ WRONG: Wrong icon for model
"model": "gemini-2.5-pro",
"icon": "/icons/openai-icon.svg"

// ❌ WRONG: Missing provider
"config": {}  // Missing provider field

// ❌ WRONG: Case-sensitive mismatch
"model": "GEMINI-2.5-PRO"  // Should be lowercase

// ✅ CORRECT: Valid model node
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

---

## Quick Reference Table

| Model | Model ID | Icon | Provider |
|-------|----------|------|----------|
| DeepSeek V3.1 | `accounts/fireworks/models/deepseek-v3p1` | `/icons/deepseek-icon.svg` | CHAT_FIREWORKS |
| Kimi K2 Instruct | `accounts/fireworks/models/kimi-k2-instruct-0905` | `/icons/kimi.svg` | CHAT_FIREWORKS |
| Kimi K2 Thinking | `accounts/fireworks/models/kimi-k2-thinking` | `/icons/kimi.svg` | CHAT_FIREWORKS |
| DeepSeek V3.1 Terminus | `accounts/fireworks/models/deepseek-v3p1-terminus` | `/icons/deepseek-icon.svg` | CHAT_FIREWORKS |
| GPT-OSS 120B (Fireworks) | `accounts/fireworks/models/gpt-oss-120b` | `/icons/openai-icon.svg` | CHAT_FIREWORKS |
| Llama 3.3 70B | `llama-3.3-70b` | `/icons/meta-icon.svg` | CHAT_CEREBRAS |
| GPT-OSS 120B (Cerebras) | `gpt-oss-120b` | `/icons/openai-icon.svg` | CHAT_CEREBRAS |
| Qwen 3 32B | `qwen-3-32b` | `/icons/qwen-icon.svg` | CHAT_CEREBRAS |
| Gemini 2.5 Pro | `gemini-2.5-pro` | `/icons/gemini.png` | GOOGLE |
| Gemini 2.5 Flash | `gemini-2.5-flash` | `/icons/gemini.png` | GOOGLE |

---

## Notes

- **API Keys**: Should be injected at runtime, not hardcoded in JSON
- **Model IDs**: Are case-sensitive - use exact values from registry
- **Icons**: Must match the model's provider for consistent UI
- **Provider**: Must match the exact provider name (CHAT_FIREWORKS, CHAT_CEREBRAS, GOOGLE)