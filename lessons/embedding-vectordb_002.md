---
## 📚 Lesson #002
**Topic:** Connecting Embedding Models to Vector Databases
**Date:** 2025-01-07
**Difficulty:** Beginner
**Tags:** #embeddings #vector-database #chroma #practical
---

### 📌 Quick Summary

Yes! You generate vectors with an embedding model, then store them in a vector database. The flow is: **Text → Embedding Model → Vector → Vector DB → Search**

### 🤔 Why It Matters

This is the core pattern for RAG, semantic search, and any AI application that needs to "remember" information.

### 📖 The Complete Flow

```
┌────────────────────────────────────────────────────────────┐
│                    COMPLETE FLOW                            │
├────────────────────────────────────────────────────────────┤
│                                                             │
│  "I love Python"                                           │
│        │                                                    │
│        ▼                                                    │
│  ┌──────────────────┐                                      │
│  │  Embedding Model │  (OpenAI, BGE, etc.)                 │
│  └────────┬─────────┘                                      │
│           │                                                 │
│           ▼                                                 │
│  [0.12, -0.34, 0.56, ...]  ← Vector (1536 numbers)        │
│           │                                                 │
│           ▼                                                 │
│  ┌──────────────────┐                                      │
│  │   Vector DB      │  (Chroma, Pinecone, etc.)           │
│  │   STORE & INDEX  │                                      │
│  └──────────────────┘                                      │
│                                                             │
└────────────────────────────────────────────────────────────┘
```

### 💻 Code Examples

#### Example 1: Manual Flow (Step by Step)

```python
# Step 1: Generate embedding with OpenAI
from openai import OpenAI
import chromadb

client = OpenAI()

# Generate embedding manually
text = "I love Python programming"
response = client.embeddings.create(
    model="text-embedding-3-small",
    input=text
)
vector = response.data[0].embedding  # [0.12, -0.34, ...]

# Step 2: Store in Chroma
chroma = chromadb.Client()
collection = chroma.create_collection("my_docs")

# Store the vector we generated
collection.add(
    ids=["doc1"],
    embeddings=[vector],  # ← Our vector goes here!
    documents=[text],     # ← Original text for reference
    metadatas=[{"source": "manual"}]
)

# Step 3: Search (also need to embed the query!)
query = "Python coding"
query_response = client.embeddings.create(
    model="text-embedding-3-small",  # SAME model!
    input=query
)
query_vector = query_response.data[0].embedding

results = collection.query(
    query_embeddings=[query_vector],
    n_results=1
)
print(results["documents"])  # [['I love Python programming']]
```

#### Example 2: Chroma Auto-Embedding (Simpler!)

```python
import chromadb

# Chroma can auto-generate embeddings for you!
chroma = chromadb.Client()
collection = chroma.create_collection("auto_embed")

# Just add text - Chroma handles embedding internally
collection.add(
    ids=["doc1", "doc2", "doc3"],
    documents=[
        "I love Python programming",
        "JavaScript is fun",
        "I enjoy cooking pasta"
    ]
)

# Query with text - Chroma auto-embeds query too
results = collection.query(
    query_texts=["coding in Python"],  # Auto-embedded!
    n_results=2
)
print(results["documents"])
# [['I love Python programming', 'JavaScript is fun']]
```

#### Example 3: Custom Embedding Model with Chroma

```python
import chromadb
from chromadb.utils import embedding_functions

# Use OpenAI embeddings with Chroma
openai_ef = embedding_functions.OpenAIEmbeddingFunction(
    api_key="YOUR_API_KEY",
    model_name="text-embedding-3-small"
)

chroma = chromadb.Client()
collection = chroma.create_collection(
    "with_openai",
    embedding_function=openai_ef  # ← Use OpenAI instead of default
)

# Now Chroma uses OpenAI to embed
collection.add(
    ids=["doc1"],
    documents=["I love Python programming"]
)

results = collection.query(
    query_texts=["Python coding"],
    n_results=1
)
```

#### Example 4: LangChain (Most Elegant)

```python
from langchain_openai import OpenAIEmbeddings
from langchain_chroma import Chroma

# Create embedding model
embeddings = OpenAIEmbeddings(model="text-embedding-3-small")

# Create vector store with embedding model
vectorstore = Chroma.from_texts(
    texts=[
        "I love Python programming",
        "JavaScript is fun",
        "I enjoy cooking pasta"
    ],
    embedding=embeddings,  # ← LangChain handles the connection!
    persist_directory="./chroma_db"
)

# Search
results = vectorstore.similarity_search("Python coding", k=2)
for doc in results:
    print(doc.page_content)
# I love Python programming
# JavaScript is fun
```

### ⚠️ Common Pitfalls

| Pitfall | Solution |
|---------|----------|
| Different models for index vs query | **Always use the SAME embedding model** |
| Forgetting to embed the query | Query must also be a vector |
| Dimension mismatch | Index dimensions must match model output |

### ✅ Key Takeaways

```
Text ──→ Embedding Model ──→ Vector ──→ Vector DB
                                            │
Query ──→ Embedding Model ──→ Vector ──────→ Search
          (SAME model!)
```

1. **Embedding Model** = Converts text to vectors
2. **Vector DB** = Stores and searches vectors
3. **Same Model Rule** = Index and query must use identical model
4. **LangChain/Chroma** = Can auto-handle the connection for you
