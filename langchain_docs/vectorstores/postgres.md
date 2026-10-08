# LangChain Postgres Vector Store Reference

> Source: https://reference.langchain.com/python/langchain-postgres

## Overview

`langchain-postgres` provides PostgreSQL-backed vector storage for LangChain applications. It supports both synchronous (`PGVector`) and asynchronous (`AsyncPGVectorStore`) operations with multiple index types (HNSW, IVFFlat, Exact Nearest Neighbor).

## Package Structure

```
langchain_postgres/
├── vectorstores/         # Main vector store implementations
├── v2/
│   ├── vectorstores/     # V2 vector stores (AsyncPGVectorStore, PGVectorStore)
│   ├── indexes/          # Index implementations (HNSW, IVFFlat, Exact)
│   ├── engine/           # Database engine (PGEngine)
│   └── translator/       # Query translation
├── chat_message_histories/  # Chat history storage
└── _utils/               # Utility functions
```

---

## Classes

### V1 Vector Store (Legacy)

| Class | Module | Description |
|-------|--------|-------------|
| `PGVector` | `langchain_postgres.vectorstores` | Main synchronous vector store |
| `DistanceStrategy` | `langchain_postgres.vectorstores` | Distance metric (COSINE, EUCLIDEAN, etc.) |
| `PGVectorTranslator` | `langchain_postgres.translator` | Translate queries to SQL |
| `DBConnection` | `langchain_postgres.vectorstores` | Database connection config |
| `Matrix` | `langchain_postgres._utils` | Matrix type for embeddings |

### V2 Vector Stores (Recommended)

| Class | Module | Description |
|-------|--------|-------------|
| `PGVectorStore` | `langchain_postgres.v2.vectorstores` | Synchronous V2 vector store |
| `AsyncPGVectorStore` | `langchain_postgres.v2.async_vectorstore` | Asynchronous V2 vector store |
| `PGEngine` | `langchain_postgres.v2.engine` | Database engine manager |
| `Column` | `langchain_postgres.v2.engine` | Column definition |
| `ColumnDict` | `langchain_postgres.v2.engine` | Column dictionary |

### Index Types (V2)

| Class | Module | Description |
|-------|--------|-------------|
| `BaseIndex` | `langchain_postgres.v2.indexes` | Base index class |
| `ExactNearestNeighbor` | `langchain_postgres.v2.indexes` | Exact KNN search (no index) |
| `HNSWIndex` | `langchain_postgres.v2.indexes` | HNSW approximate index |
| `IVFFlatIndex` | `langchain_postgres.v2.indexes` | IVF-Flat approximate index |
| `DistanceStrategy` | `langchain_postgres.v2.indexes` | Distance metric for indexes |
| `StrategyMixin` | `langchain_postgres.v2.indexes` | Strategy mixin |
| `QueryOptions` | `langchain_postgres.v2.indexes` | Base query options |
| `HNSWQueryOptions` | `langchain_postgres.v2.indexes` | HNSW-specific query options |
| `IVFFlatQueryOptions` | `langchain_postgres.v2.indexes` | IVFFlat-specific query options |

### Chat Message History

| Class | Module | Description |
|-------|--------|-------------|
| `PostgresChatMessageHistory` | `langchain_postgres.chat_message_histories` | PostgreSQL-backed chat history |

---

## Functions

### Utilities

| Function | Module | Description |
|----------|--------|-------------|
| `cosine_similarity()` | `langchain_postgres._utils` | Compute cosine similarity |
| `maximal_marginal_relevance()` | `langchain_postgres._utils` | MMR for diverse results |
| `validate_identifier()` | `langchain_postgres.v2.indexes` | Validate index identifiers |

---

## Distance Strategies

```python
from langchain_postgres.vectorstores import DistanceStrategy
from langchain_postgres.v2.indexes import DistanceStrategy as V2DistanceStrategy

# V1
DistanceStrategy.COSINE      # Cosine similarity (default)
DistanceStrategy.EUCLIDEAN   # Euclidean distance
DistanceStrategy.MAX_INNER_PRODUCT  # Max inner product

# V2 (same values)
V2DistanceStrategy.COSINE
V2DistanceStrategy.EUCLIDEAN
V2DistanceStrategy.MAX_INNER_PRODUCT
```

---

## Quick Start (V2 - Recommended)

### Synchronous

```python
from langchain_postgres.v2 import PGVectorStore, PGEngine
from langchain_openai import OpenAIEmbeddings

# Create engine
engine = PGEngine.from_connection_string(
    "postgresql+psycopg://user:pass@localhost:5432/db"
)

# Create vector store
embeddings = OpenAIEmbeddings()
store = PGVectorStore.create(
    engine=engine,
    table_name="my_vectors",
    embeddings=embeddings,
    distance_strategy="cosine"
)

# Add documents
from langchain_core.documents import Document
docs = [Document(page_content="text", metadata={"source": "doc1"})]
store.add_documents(docs)

# Search
results = store.similarity_search("query", k=5)
```

### Asynchronous

```python
from langchain_postgres.v2 import AsyncPGVectorStore, PGEngine
from langchain_openai import OpenAIEmbeddings

engine = PGEngine.from_connection_string(
    "postgresql+psycopg://user:pass@localhost:5432/db"
)

store = await AsyncPGVectorStore.create(
    engine=engine,
    table_name="my_vectors",
    embeddings=OpenAIEmbeddings(),
)

# Async operations
await store.aadd_documents(docs)
results = await store.asimilarity_search("query", k=5)
```

---

## Index Creation

### HNSW Index (Recommended for large datasets)

```python
from langchain_postgres.v2.indexes import HNSWIndex, HNSWQueryOptions

# Create HNSW index
index = HNSWIndex(
    name="hnsw_idx",
    distance_strategy=DistanceStrategy.COSINE,
    m=16,              # Max connections per layer
    ef_construction=64 # Construction ef
)

store.create_index(index)

# Query with HNSW options
results = store.similarity_search(
    "query",
    k=5,
    query_options=HNSWQueryOptions(ef_search=128)
)
```

### IVFFlat Index

```python
from langchain_postgres.v2.indexes import IVFFlatIndex, IVFFlatQueryOptions

index = IVFFlatIndex(
    name="ivfflat_idx",
    distance_strategy=DistanceStrategy.COSINE,
    lists=100  # Number of clusters
)

store.create_index(index)

results = store.similarity_search(
    "query",
    k=5,
    query_options=IVFFlatQueryOptions(probes=10)
)
```

### Exact Nearest Neighbor (Small datasets)

```python
from langchain_postgres.v2.indexes import ExactNearestNeighbor

index = ExactNearestNeighbor(
    name="exact_idx",
    distance_strategy=DistanceStrategy.COSINE
)

store.create_index(index)
```

---

## Advanced Configuration

### Custom Schema

```python
from langchain_postgres.v2.vectorstores import PGVectorStore
from langchain_postgres.v2.engine import Column

# Define custom columns
columns = [
    Column("id", "UUID", primary_key=True),
    Column("embedding", "VECTOR(1536)"),
    Column("document", "TEXT"),
    Column("metadata", "JSONB"),
    Column("custom_field", "VARCHAR(255)"),
]

store = PGVectorStore(
    engine=engine,
    table_name="custom_vectors",
    embedding_column="embedding",
    document_column="document",
    metadata_column="metadata",
    columns=columns
)
```

### Filter by Metadata

```python
# Filter during search
results = store.similarity_search(
    "query",
    k=5,
    filter={"source": "doc1", "category": "tech"}
)

# With async
results = await store.asimilarity_search(
    "query",
    k=5,
    filter={"user_id": "123"}
)
```

### MMR (Maximal Marginal Relevance)

```python
# Diverse results
results = store.max_marginal_relevance_search(
    "query",
    k=5,
    fetch_k=20,  # Fetch more candidates
    lambda_mult=0.5  # Diversity vs relevance
)
```

---

## Connection Pooling

```python
from langchain_postgres.v2 import PGEngine

engine = PGEngine.from_connection_string(
    "postgresql+psycopg://user:pass@localhost:5432/db",
    pool_size=10,
    max_overflow=20,
    pool_timeout=30,
    pool_recycle=3600
)
```

---

## Migration from V1 to V2

```python
# V1 (Legacy)
from langchain_postgres import PGVector
store = PGVector(
    connection_string="postgresql://...",
    embedding_function=embeddings,
    collection_name="my_collection"
)

# V2 (New)
from langchain_postgres.v2 import PGVectorStore, PGEngine
engine = PGEngine.from_connection_string("postgresql+psycopg://...")
store = PGVectorStore.create(
    engine=engine,
    table_name="my_collection",  # Same as collection_name
    embeddings=embeddings
)
```

---

## Schema Requirements

The vector store creates tables with:

```sql
-- V2 Default schema
CREATE TABLE IF NOT EXISTS my_vectors (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    embedding VECTOR(1536),  -- Dimension matches embeddings
    document TEXT,
    metadata JSONB,
    -- Custom columns as defined
);

-- Indexes (created separately)
CREATE INDEX hnsw_idx ON my_vectors USING hnsw (embedding vector_cosine_ops);
CREATE INDEX ivfflat_idx ON my_vectors USING ivfflat (embedding vector_cosine_ops) WITH (lists = 100);
```

---

## Requirements

- PostgreSQL 14+ with `pgvector` extension
- `psycopg` (v3) or `psycopg2` driver
- `pgvector` Python package

```bash
# Install
pip install langchain-postgres pgvector psycopg

# Enable pgvector in PostgreSQL
CREATE EXTENSION vector;
```

---

## Performance Tips

1. **Use HNSW for production** - Best balance of speed/accuracy
2. **Tune `ef_search`** - Higher = more accurate, slower
3. **Use connection pooling** - Critical for concurrent access
4. **Batch inserts** - Use `add_documents` with lists
5. **Filter early** - Apply metadata filters before vector search
6. **Monitor index size** - HNSW uses more memory than IVFFlat

---

## Related Documentation

- [LangChain Package](../langchain/README.md) - Main package
- [LangGraph](../langgraph/README.md) - For agent memory
- [Deep Agents](../deepagents/README.md) - For long-term memory