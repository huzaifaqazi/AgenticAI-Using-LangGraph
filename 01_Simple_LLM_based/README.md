# Simple LLM-based Workflow — LangGraph Fundamentals

## 📌 Overview

This notebook demonstrates the **simplest LLM-powered workflow** in LangGraph — a single node that takes a question and returns an answer using Groq's Llama 3.1 model. Despite its simplicity, it introduces the three core building blocks of any LangGraph application.

## 🧱 Core Concepts

### 1. State (`chain_state`)

**What it is:** A typed dictionary (`TypedDict`) that holds all the data flowing through your graph. Think of it as the "memory" or "shared context" of your workflow.

```python
class chain_state(TypedDict):
    question: str   # input
    answer: str     # output
```

**Why we use it:**
- ✅ Centralizes data — every node reads from and writes to the same state
- ✅ Type-safe — you know exactly what fields are available at each step
- ✅ Makes the flow predictable and easy to debug

### 2. Nodes

**What it is:** A node is a **function** that performs one unit of work. It receives the current state and returns an **updated state** (or a partial update).

```python
def question_answer(state: chain_state):
    # take question, call LLM, return answer
    return {"answer": llm_response}
```

**Why we use it:**
- ✅ Encapsulates logic — each node does exactly one thing
- ✅ Reusable — you can mix and match nodes across different graphs
- ✅ Testable — you can call a node function directly with mock state

### 3. Edges

**What it is:** An edge defines **how control flows** between nodes — i.e., which node runs next, and in what order.

```python
graph.add_edge(START, "question_answer")
graph.add_edge("question_answer", END)
```

**Why we use it:**
- ✅ Decouples flow from logic — nodes don't know about each other
- ✅ Enables complex patterns: branching, looping, conditional routing
- ✅ Makes the execution path visible and auditable

## 🔗 How They Work Together

```
[START] 
    │
    ▼
┌─────────────────────┐
│  Node: question_answer │  ← reads from state, calls LLM, writes to state
└─────────────────────┘
    │
    ▼
  [END]
```

The **State** flows through each **Node** in the order defined by **Edges**. Every node can read any field from the state and write back updates. When a node finishes, the graph follows the edges to decide what runs next.

## ❓ Why Use LangGraph Instead of Simple Function Calls?

| LangGraph | Plain Python |
|---|---|
| Built-in state management | Manual variable passing |
| Visual graph representation | No visibility of flow |
| Conditional/loop routing | Requires manual `if/for` logic |
| Human-in-the-loop support | Hard to pause/resume |
| Easy to extend with new nodes | Requires refactoring |

Even for simple workflows, LangGraph gives you a **structured, scalable foundation** — you can start with one node and grow to dozens without rewriting your code.
