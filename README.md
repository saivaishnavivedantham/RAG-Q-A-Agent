# RAG-Q-A-Agent
Certainly! Below is a **clean, copy-paste-ready `README.md`** version with no emojis, formatted for GitHub. You can copy it directly into a file named `README.md` in your 
# Financial Q&A Agent using RAG and 10-K Filings

This repository implements a Retrieval-Augmented Generation (RAG) pipeline with agent capabilities to answer financial questions about Google, Microsoft, and NVIDIA based on their 10-K filings (2022–2024).

The system supports basic, comparative, cross-company, segment-level, and AI strategy questions using query decomposition and multi-step reasoning.

---

## Features

- RAG-based semantic search over SEC 10-K filings
- Query decomposition using Groq LLM
- Multi-step semantic retrieval and synthesis
- Structured JSON output including answer, sub-queries, reasoning, and sources
- Implemented in Jupyter Notebook using sentence-transformers and Groq API

---

## Directory Structure

```

.
├── main.ipynb # Entry notebook to run end-to-end agent
├── agent_query_engine.ipynb # Core logic for query decomposition and RAG answering
├── embedder.ipynb # Embeds chunked text using sentence-transformers
├── chunker.ipynb # Splits extracted 10-K text into semantic chunks
├── extractor.ipynb # Parses and extracts text from 10-K filings
├── downloader.ipynb # (Optional) Script to download 10-Ks from EDGAR
├── requirements.txt # Dependencies for this project

├── data/
│   ├── embedded_chunks.json         # Final embedded vector chunks
│   └── extracted/
│       └── NVDA_2024_10K.txt        # Example raw text of 10-K after extraction

├── chunks/
│   └── all_chunks.json              # Chunked text before embedding

├── sample_outputs/
    └── ai_investments_comparison_2024.json  # Example JSON output for testing

````

## Supported Query Types

| Query Type       | Example                                                                 |
|------------------|-------------------------------------------------------------------------|
| Basic            | What was Microsoft's total revenue in 2023?                            |
| Year-over-Year   | How did NVIDIA’s data center revenue grow from 2022 to 2023?           |
| Cross-Company    | Which company had the highest operating margin in 2023?                |
| Segment-Level    | What percentage of Google’s revenue came from cloud in 2023?           |
| AI Strategy      | Compare AI investments mentioned by all three companies in 2024.       |

---

## Setup Instructions

### 1. Clone the repository

```bash
git clone https://github.com/your-username/financial-rag-agent.git
cd financial-rag-agent
````

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Set your Groq API key

In `agent_query_engine.ipynb`, locate this line and replace it with your actual Groq API key:

```python
client = Groq(api_key="YOUR_GROQ_API_KEY")
```

### 4. (Optional) Run preprocessing pipeline

Only needed if you don't use pre-embedded data:

```bash
python utils/downloader.py
python utils/extractor.py
python utils/chunker.py
python utils/embedder.py
```

---

## Running the Agent

Launch `agent_query_engine.ipynb` in Jupyter. Run the notebook cells sequentially to:

* Decompose user queries into sub-queries
* Embed and retrieve relevant 10-K chunks
* Generate answers using Groq's LLM
* Display answer, reasoning, and source excerpts

---

## Example Output

```json
{
  "query": "Which company had the highest operating margin in 2023?",
  "answer": "Microsoft had the highest operating margin at 42.1%...",
  "reasoning": "Retrieved operating margins for all three companies and compared.",
  "sub_queries": [
    "Microsoft operating margin 2023",
    "Google operating margin 2023",
    "NVIDIA operating margin 2023"
  ],
  "sources": [
    {
      "company": "Microsoft",
      "year": "2023",
      "excerpt": "Operating margin was 42.1%...",
      "score": 0.88
    }
  ]
}
```

---

## Requirements

* Python 3.8+
* Packages: `groq`, `sentence-transformers`, `numpy`, `scikit-learn`, `PyPDF2`, `pdfplumber`, etc.

---

## References

* [SEC EDGAR Filings](https://www.sec.gov/edgar.shtml)
* [SentenceTransformers](https://www.sbert.net/)
* [Groq API](https://console.groq.com/)

---

## Sample Queries to Try

```python
queries = [
    "What was Microsoft's total revenue in 2023?",
    "How did NVIDIA's data center revenue grow from 2022 to 2023?",
    "Which company had the highest operating margin in 2023?",
    "What percentage of Google's revenue came from cloud in 2023?",
    "Compare AI investments mentioned by all three companies in 2024."
]
```
