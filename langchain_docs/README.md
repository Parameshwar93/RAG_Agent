# LangChain Documentation for RAG

This directory contains extracted reference documentation for LangChain packages relevant to building RAG (Retrieval-Augmented Generation) systems.

## Package Index

| Package | Description | Path |
|---------|-------------|------|
| **langchain** | Main LangChain package - agents, models, tools, middleware | [langchain/README.md](langchain/README.md) |
| **langgraph** | Graph-based orchestration for stateful agents | [langgraph/README.md](langgraph/README.md) |
| **deepagents** | Deep agents with planning, subagents, filesystem, memory | [deepagents/README.md](deepagents/README.md) |
| **middlewares** | LangChain middleware reference (retry, HITL, PII, etc.) | [middlewares/README.md](middlewares/README.md) |
| **vectorstores/postgres** | PostgreSQL vector store (PGVector, AsyncPGVectorStore) | [vectorstores/postgres.md](vectorstores/postgres.md) |

---

## Quick Start for RAG

### 1. Basic RAG with LangChain + PGVector

```python
from langchain_postgres.v2 import PGVectorStore, PGEngine
from langchain_openai import OpenAIEmbeddings, ChatOpenAI
from langchain import create_agent
from langchain_core.documents import Document

# Setup vector store
engine = PGEngine.from_connection_string("postgresql+psycopg://...")
embeddings = OpenAIEmbeddings()
vector_store = PGVectorStore.create(
    engine=engine,
    table_name="rag_docs",
    embeddings=embeddings
)

# Add documents
docs = [Document(page_content=text, metadata={"source": "doc1"}) for text in documents]
vector_store.add_documents(docs)

# Create retriever
retriever = vector_store.as_retriever(search_kwargs={"k": 5})

# Create RAG agent
model = ChatOpenAI(model="gpt-4o-mini")
agent = create_agent(model=model, tools=[retriever])
```

### 2. Advanced RAG with LangGraph

```python
from langgraph.graph import StateGraph, MessagesState
from langgraph.func import entrypoint
from langchain_postgres.v2 import AsyncPGVectorStore

class RAGState(MessagesState):
    query: str
    context: list

@entrypoint()
async def retrieve(state: RAGState):
    store = await AsyncPGVectorStore.create(engine=engine, ...)
    results = await store.asimilarity_search(state["query"], k=5)
    return {"context": results}

@entrypoint()
async def generate(state: RAGState):
    # Use context to generate answer
    context = "\n".join([d.page_content for d in state["context"]])
    prompt = f"Answer based on:\n{context}\n\nQuestion: {state['query']}"
    response = await model.ainvoke(prompt)
    return {"messages": [response]}

# Build graph
graph = StateGraph(RAGState)
graph.add_node("retrieve", retrieve)
graph.add_node("generate", generate)
graph.set_entry_point("retrieve")
graph.add_edge("retrieve", "generate")
app = graph.compile()
```

### 3. Deep Agents for Complex RAG

```python
from deepagents import create_deep_agent
from deepagents.middleware.memory import MemoryMiddleware
from deepagents.middleware.filesystem import FilesystemMiddleware

agent = create_deep_agent(
    model=model,
    tools=[retriever, web_search, code_executor],
    instructions="""
    You are a research agent. For complex questions:
    1. Break down the question
    2. Use subagents for parallel research
    3. Synthesize findings
    4. Cite sources
    """,
    middleware=[
        MemoryMiddleware(),           # Long-term memory
        FilesystemMiddleware(),       # File operations
    ],
    subagents=[
        create_sub_agent(name="researcher", description="Deep research"),
        create_sub_agent(name="analyst", description="Data analysis"),
    ]
)
```

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                      Your Application                        │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│   LangChain   │    │   LangGraph   │    │  Deep Agents  │
│  (High-level) │    │ (Orchestration)│    │ (Complex tasks)│
└───────┬───────┘    └───────┬───────┘    └───────┬───────┘
        │                    │                    │
        └────────────────────┼────────────────────┘
                             ▼
              ┌───────────────────────────────┐
              │      Vector Store Layer        │
              │  ┌─────────────────────────┐  │
              │  │   langchain-postgres     │  │
              │  │   (PGVector / Async)     │  │
              │  └─────────────────────────┘  │
              └───────────────────────────────┘
                             │
                             ▼
              ┌───────────────────────────────┐
              │      PostgreSQL + pgvector     │
              └───────────────────────────────┘
```

---

## Middleware for RAG

Key middlewares that enhance RAG systems:

| Middleware | Purpose | RAG Use Case |
|------------|---------|--------------|
| `ModelRetryMiddleware` | Retry failed LLM calls | Handle API rate limits |
| `SummarizationMiddleware` | Compress conversation history | Manage context window |
| `HumanInTheLoopMiddleware` | Human review of retrieval | Quality control |
| `PIIMiddleware` | Redact sensitive data | Privacy compliance |
| `MemoryMiddleware` (Deep Agents) | Long-term memory | Multi-session RAG |
| `SubAgentMiddleware` (Deep Agents) | Parallel sub-tasks | Multi-hop retrieval |

---

## Vector Store Comparison

| Feature | PGVector (V1) | PGVectorStore (V2) | AsyncPGVectorStore |
|---------|---------------|---------------------|-------------------|
| **API** | Sync only | Sync | Async |
| **Indexes** | HNSW, IVFFlat | HNSW, IVFFlat, Exact | HNSW, IVFFlat, Exact |
| **Filtering** | Basic metadata | Advanced metadata | Advanced metadata |
| **Connection Pool** | Manual | Built-in | Built-in |
| **Recommended** | Legacy | ✅ Production | ✅ High concurrency |

---

## Installation

```bash
# Core packages
pip install langchain langgraph deepagents

# Vector store
pip install langchain-postgres pgvector psycopg

# Optional: OpenAI
pip install langchain-openai
```

---

## PostgreSQL Setup

```sql
-- Enable pgvector extension
CREATE EXTENSION IF NOT EXISTS vector;

-- Optional: Create dedicated user
CREATE USER rag_user WITH PASSWORD 'secure_password';
GRANT ALL PRIVILEGES ON DATABASE rag_db TO rag_user;
```

---

## Further Reading

- [LangChain Official Docs](https://docs.langchain.com)
- [LangGraph Docs](https://docs.langchain.com/langgraph)
- [Deep Agents Guide](https://docs.langchain.com/deepagents)
- [PGVector Documentation](https://github.com/pgvector/pgvector)

---

*Generated from https://reference.langchain.com/?lang=python*