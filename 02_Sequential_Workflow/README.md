# Sequential Workflow — Prompt Chaining

## 📌 What is Prompt Chaining?

Prompt chaining is a pattern where a task is broken into multiple sequential steps, and the output of one LLM call becomes the input (or part of the context) for the next. Instead of asking a model to do everything in one massive prompt, you split the work into smaller, focused sub-tasks — each with its own clear instruction.

Think of it like an assembly line: each station (node) does one thing well and passes the result to the next station.

## 🔁 How This Notebook Works

This workflow implements a **3-step content generation pipeline** using LangGraph:

```
Topic → [Generate Outline] → Outline → [Generate Blog] → Blog → [Score Blog] → Evaluation
```

| Step | Node | What it does |
|---|---|---|
| 1 | `Generate_Outline` | Takes a topic and produces a structured outline |
| 2 | `Generate_Blog` | Takes the topic + outline and writes a full blog post |
| 3 | `Grade_myblog_Score` | Takes the outline + blog and evaluates the blog's quality (score out of 10) |

Each node is a separate LLM call with a focused prompt. The shared state (`chain_state`) carries `topic → outline → content → Score` through the graph.

## ❓ Why Use Prompt Chaining?

| Benefit | Explanation |
|---|---|
| ✅ **Better quality** | Each step has a focused task — the model doesn't get overwhelmed by complexity |
| ✅ **Debugging** | You can inspect the output of each step individually to find where things go wrong |
| ✅ **Cost control** | Shorter, focused prompts use fewer tokens per call than one giant prompt |
| ✅ **Modularity** | Easy to swap, add, or remove steps without rewriting everything |
| ✅ **Accountability** | Each node has a single responsibility — clear inputs and outputs |

## 🧠 When to Use It

Prompt chaining is ideal when your task has a **natural sequence of sub-tasks**:

- Content generation (outline → draft → edit → summarize)
- Data extraction (find → validate → format)
- Multi-step reasoning (analyze → conclude → recommend)
- RAG pipelines (retrieve → filter → answer)

---

> 💡 **Tip:** If you find yourself writing a prompt with "First do X, then do Y, then do Z", consider refactoring it into a chain of focused prompts.
