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