---
## 📚 Lesson #001
**Topic:** Creating LangChain Agents and Connecting with LangGraph
**Date:** 2025-01-07
**Difficulty:** Intermediate
**Tags:** #langchain #langgraph #agents #workflow #orchestration
---

### 📌 Quick Summary

Create agents using LangChain (building blocks), then wire them together using LangGraph (orchestration). LangChain builds individual agents; LangGraph connects them into a workflow with state, loops, and conditional branching.

### 🤔 Why It Matters

Complex AI applications need multiple specialized agents working together. LangGraph lets you orchestrate these agents with proper state management, conditional logic, and the ability to loop back for revisions.

---

## 🎯 The Pattern

```
┌──────────────────────────────────────────────────────────────────────────┐
│                                                                           │
│   LANGCHAIN                           LANGGRAPH                          │
│   (Build Agents)                      (Connect Agents)                   │
│                                                                           │
│   ┌─────────────┐                     ┌─────────────────────────────┐   │
│   │  Agent 1    │──────┐              │                             │   │
│   │ (Researcher)│      │              │   Agent1 ──▶ Agent2 ──▶    │   │
│   └─────────────┘      │              │      │          │          │   │
│                        │              │      │          ▼          │   │
│   ┌─────────────┐      ├───build───▶  │      │       Agent3        │   │
│   │  Agent 2    │      │     flow     │      │          │          │   │
│   │  (Writer)   │──────┤              │      └──◀───────┘ (loop)   │   │
│   └─────────────┘      │              │                             │   │
│                        │              └─────────────────────────────┘   │
│   ┌─────────────┐      │                                                │
│   │  Agent 3    │──────┘                                                │
│   │ (Reviewer)  │                                                       │
│   └─────────────┘                                                       │
│                                                                           │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 Complete Code Example

```python
from langchain_openai import ChatOpenAI
from langchain.agents import create_tool_calling_agent, AgentExecutor
from langchain.tools import tool
from langchain_core.prompts import ChatPromptTemplate
from langgraph.graph import StateGraph, END
from typing import TypedDict, Annotated
import operator

# ═══════════════════════════════════════════════════════════════════════════
# STEP 1: Create 3 Agents using LangChain
# ═══════════════════════════════════════════════════════════════════════════

llm = ChatOpenAI(model="gpt-4")

# ─────────────────────────────────────────────────
# Agent 1: Researcher (searches for information)
# ─────────────────────────────────────────────────
@tool
def search_web(query: str) -> str:
    """Search the web for information."""
    # In real app, use actual search API
    return f"Search results for '{query}': [mock data about the topic]"

researcher_prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a researcher. Use the search tool to find information."),
    ("human", "{input}"),
    ("placeholder", "{agent_scratchpad}")
])

researcher_agent = create_tool_calling_agent(llm, [search_web], researcher_prompt)
researcher_executor = AgentExecutor(agent=researcher_agent, tools=[search_web])

# ─────────────────────────────────────────────────
# Agent 2: Writer (writes content based on research)
# ─────────────────────────────────────────────────
@tool
def format_document(content: str) -> str:
    """Format content into a structured document."""
    return f"## Formatted Document\n\n{content}"

writer_prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a writer. Write clear, engaging content based on research provided."),
    ("human", "{input}"),
    ("placeholder", "{agent_scratchpad}")
])

writer_agent = create_tool_calling_agent(llm, [format_document], writer_prompt)
writer_executor = AgentExecutor(agent=writer_agent, tools=[format_document])

# ─────────────────────────────────────────────────
# Agent 3: Reviewer (reviews and approves content)
# ─────────────────────────────────────────────────
@tool
def check_quality(content: str) -> str:
    """Check content quality and return feedback."""
    if len(content) < 100:
        return "NEEDS_REVISION: Content is too short"
    return "APPROVED: Content meets quality standards"

reviewer_prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a reviewer. Check content quality and decide if it needs revision."),
    ("human", "{input}"),
    ("placeholder", "{agent_scratchpad}")
])

reviewer_agent = create_tool_calling_agent(llm, [check_quality], reviewer_prompt)
reviewer_executor = AgentExecutor(agent=reviewer_agent, tools=[check_quality])


# ═══════════════════════════════════════════════════════════════════════════
# STEP 2: Create LangGraph Flow using the 3 Agents
# ═══════════════════════════════════════════════════════════════════════════

# Define shared state
class WorkflowState(TypedDict):
    topic: str
    research: str
    draft: str
    review: str
    is_approved: bool
    revision_count: int

# ─────────────────────────────────────────────────
# Wrap agents as LangGraph nodes
# ─────────────────────────────────────────────────
def research_node(state: WorkflowState) -> WorkflowState:
    """Node 1: Researcher agent"""
    result = researcher_executor.invoke({
        "input": f"Research this topic: {state['topic']}"
    })
    state["research"] = result["output"]
    return state

def writer_node(state: WorkflowState) -> WorkflowState:
    """Node 2: Writer agent"""
    result = writer_executor.invoke({
        "input": f"Write about this based on research:\n{state['research']}"
    })
    state["draft"] = result["output"]
    return state

def reviewer_node(state: WorkflowState) -> WorkflowState:
    """Node 3: Reviewer agent"""
    result = reviewer_executor.invoke({
        "input": f"Review this content:\n{state['draft']}"
    })
    state["review"] = result["output"]
    state["is_approved"] = "APPROVED" in result["output"]
    state["revision_count"] = state.get("revision_count", 0) + 1
    return state

# ─────────────────────────────────────────────────
# Conditional routing (the power of LangGraph!)
# ─────────────────────────────────────────────────
def should_continue(state: WorkflowState) -> str:
    if state["is_approved"]:
        return "end"
    if state["revision_count"] >= 3:  # Max 3 revisions
        return "end"
    return "revise"

# ─────────────────────────────────────────────────
# Build the graph
# ─────────────────────────────────────────────────
workflow = StateGraph(WorkflowState)

# Add nodes (our 3 agents!)
workflow.add_node("researcher", research_node)
workflow.add_node("writer", writer_node)
workflow.add_node("reviewer", reviewer_node)

# Define the flow
workflow.set_entry_point("researcher")
workflow.add_edge("researcher", "writer")
workflow.add_edge("writer", "reviewer")

# Conditional edge - loop back if not approved!
workflow.add_conditional_edges(
    "reviewer",
    should_continue,
    {
        "revise": "researcher",  # Loop back to research more
        "end": END
    }
)

# Compile
app = workflow.compile()


# ═══════════════════════════════════════════════════════════════════════════
# STEP 3: Run the workflow!
# ═══════════════════════════════════════════════════════════════════════════

result = app.invoke({
    "topic": "Benefits of meditation",
    "research": "",
    "draft": "",
    "review": "",
    "is_approved": False,
    "revision_count": 0
})

print(result["draft"])
```

---

## 🔄 The Flow Visualized

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           YOUR WORKFLOW                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   START                                                                      │
│     │                                                                        │
│     ▼                                                                        │
│   ┌───────────────────┐                                                     │
│   │    RESEARCHER     │  ← LangChain Agent 1                                │
│   │   (search_web)    │                                                     │
│   └─────────┬─────────┘                                                     │
│             │                                                                │
│             ▼                                                                │
│   ┌───────────────────┐                                                     │
│   │      WRITER       │  ← LangChain Agent 2                                │
│   │ (format_document) │                                                     │
│   └─────────┬─────────┘                                                     │
│             │                                                                │
│             ▼                                                                │
│   ┌───────────────────┐                                                     │
│   │     REVIEWER      │  ← LangChain Agent 3                                │
│   │  (check_quality)  │                                                     │
│   └─────────┬─────────┘                                                     │
│             │                                                                │
│             ▼                                                                │
│   ┌───────────────────┐                                                     │
│   │   APPROVED?       │                                                     │
│   └─────────┬─────────┘                                                     │
│             │                                                                │
│      ┌──────┴──────┐                                                        │
│      │             │                                                         │
│     YES           NO                                                         │
│      │             │                                                         │
│      ▼             ▼                                                         │
│    END         LOOP BACK ───────────────────┐                               │
│                                              │                               │
│                                              │                               │
│   ┌───────────────────┐◀─────────────────────┘                              │
│   │    RESEARCHER     │  (research more!)                                   │
│   └───────────────────┘                                                     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### ⚠️ Common Pitfalls

| Pitfall | Solution |
|---------|----------|
| Forgetting to handle infinite loops | Add `revision_count` or max iterations |
| State not updating properly | Always return the modified state from nodes |
| Complex state management | Keep state flat, use TypedDict for type hints |

---

### ✅ Key Takeaways

| Step | What | How |
|------|------|-----|
| 1 | Create agents | **LangChain** (`create_tool_calling_agent`, `AgentExecutor`) |
| 2 | Wrap as nodes | Function that calls `executor.invoke()` and updates state |
| 3 | Connect nodes | **LangGraph** (`StateGraph`, `add_edge`, `add_conditional_edges`) |

**LangChain = Build the agents**
**LangGraph = Wire them together**
