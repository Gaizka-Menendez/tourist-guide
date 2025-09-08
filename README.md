# Virtual Tourist guide 

**Author**: *Gaizka Menéndez Hernández*

## 📌 Description
Intelligent tourist guide based on Retrieval-Augmented Generation (RAG) and LangChain agents.  
It can answer tourism-related questions only about **Barcelona, Bilbao, Galicia, Tenerife, and Valencia**.

## ⚙️ Features
- Hybrid RAG (BM25 + MMR + CrossEncoder reranking)
- Tools:
  - `hybrid_retrieve`: retrieves passages from PDF knowledge base
  - `get_weather`: fetches weather forecast via Open-Meteo API
- Memory with `MemorySaver` and `thread_id`
- Domain restrictions: refuses non-tourism or outside destinations

## 🛠️ Setup
```bash
git clone <repo>
cd <repo>
pip install -r requirements.txt



## SECTIONS:

## 1) Data Loading and Preparation
The PDF documents provided in the `data/` folder contain tourism information about the supported destinations.  
We extracted the text page by page and divided it into **chunks of 1,000 characters with 200 overlap**, ensuring each passage is manageable, preserves context, and keeps metadata (source file and page number).

---

## 2) Embeddings and Vector Store
Each chunk was converted into a **dense vector embedding** using Google GenAI.  
All vectors were stored in **FAISS**, a scalable vector database optimized for similarity search.  
This allows retrieving semantically similar passages, not only keyword matches.

---

## 3) Retrievers (Hybrid Approach)
We combined multiple retrieval strategies:
- **BM25** (lexical): effective for exact keyword matches.  
- **Dense retriever with MMR**: captures semantic meaning while avoiding redundant results.  
- **Ensemble retriever**: balances lexical (0.4) and semantic (0.6) scores.  
- **Cross-encoder reranker**: re-scores the top passages for maximum precision.  

This hybrid retrieval ensures both **coverage and accuracy** in the answers.

---

## 4) Tools
Two tools were exposed to the LLM:
- **`hybrid_retrieve`**: retrieves relevant tourism content using the hybrid pipeline.  
- **`get_weather`**: fetches weather forecasts via Open-Meteo for the five supported destinations.  

These tools allow the agent to ground its responses in **factual context** and enrich them with real-world data.

---

## 5) Agent Construction
We built a **ReAct agent** with LangGraph.  
- The agent uses Gemini as its LLM backend.  
- It decides when to call a tool and how to incorporate its output.  
- A **MemorySaver** with `thread_id` enables multi-turn conversations where context is preserved.

---

## 6) Prompting and Scope Control
The agent is guided by a **system prompt** enforcing strict rules:
- Answer only **tourism-related queries**.  
- Restrict responses to **Barcelona, Bilbao, Galicia, Tenerife, and Valencia**.  
- Use tools for factual data and politely refuse out-of-scope questions.  

This prevents hallucinations and ensures reliability.

---

## 7) Testing and Examples
The agent was tested in different scenarios:
- **Tourism queries**: “What can I see in Bilbao?” → retrieved attractions with citations.  
- **Weather queries**: “What will the weather be in Barcelona on 2025-09-10?” → forecast returned from API.  
- **Out-of-scope queries**: “What to see in Madrid?” → polite refusal explaining its scope.  

Results confirm the system responds **accurately, concisely, and within domain**.
