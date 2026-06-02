# VectorDB — Vector Database from Scratch in C++

I built a fully functional vector database in C++ with a web UI — just to understand how tools like Pinecone and Weaviate actually work under the hood. It runs three search algorithms side-by-side (HNSW, KD-Tree, Brute Force) and includes a RAG pipeline with a local LLM via Ollama.

---

## Features

| | |
|---|---|
| **3 Search Algorithms** | HNSW, KD-Tree, Brute Force — benchmark all three live |
| **3 Distance Metrics** | Cosine, Euclidean, Manhattan |
| **PCA Scatter Plot** | Watch semantic clusters form in 2D |
| **Real Embeddings** | Paste any text → Ollama converts it to a 768D vector |
| **RAG Pipeline** | Ask questions about your docs → HNSW retrieves context → local LLM answers |
| **REST API** | Full CRUD: insert, delete, search, benchmark |

---

## How It Works

```
Your Text → nomic-embed-text (768D vector) → HNSW Index → Semantic Search → llama3.2 → Answer
```

HNSW builds a multilayer graph — upper layers act like a highway to get you close fast, layer 0 zooms in. Same algorithm used by Pinecone, Weaviate, Chroma, Milvus. Achieves O(log N) vs O(N) for brute force.

---

## Tech Stack

- **C++17** — core engine (HNSW, KD-Tree, Brute Force, REST server)
- **cpp-httplib** — single-header HTTP server
- **Ollama** — local LLM inference (`nomic-embed-text` + `llama3.2`)
- **Vanilla JS** — frontend with PCA scatter plot

---

## Setup (Windows)

**Prerequisites:** MSYS2 (g++ compiler), Git, Ollama

```powershell
# 1. Install g++ via MSYS2, add C:\msys64\ucrt64\bin to PATH

# 2. Pull Ollama models
ollama pull nomic-embed-text   # 274 MB
ollama pull llama3.2            # 2 GB

# 3. Clone & compile
git clone https://github.com/YOUR_USERNAME/VectorDB.git
cd VectorDB
g++ -std=c++17 -O2 main.cpp -o db -lws2_32

# 4. Run
ollama serve        # Terminal 1
./db                # Terminal 2
```

Open **http://localhost:8080**

---

## REST API

```
GET  /search?v=f1,f2,...&k=5&metric=cosine&algo=hnsw
GET  /benchmark?v=...&k=5&metric=cosine
POST /insert
POST /doc/insert   {"title":"...", "text":"..."}
POST /doc/ask      {"question":"...", "k":3}
GET  /hnsw-info
GET  /status
```

---

## Project Structure

```
VectorDB/
├── main.cpp     ← entire backend (algorithms + API + RAG)
├── httplib.h    ← HTTP server
├── index.html   ← frontend
└── README.md
```

---

## Common Issues

| Problem | Fix |
|---|---|
| `Ollama: OFFLINE` | Run `ollama serve` |
| `g++: command not found` | Add MSYS2 bin folder to PATH |
| Port 8080 busy | `netstat -ano \| findstr 8080` → `taskkill /PID <pid> /F` |
| LLM too slow | Switch to `llama3.2:1b` — edit `genModel` in main.cpp and recompile |

---

## License

MIT
