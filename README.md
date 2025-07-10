# Financial RAG Agent on 10-K Filings (Google, Microsoft, NVIDIA)

This project implements a **Retrieval-Augmented Generation (RAG)** system that can answer complex financial questions about Microsoft, Google, and NVIDIA by using their 10-K filings from 2022 to 2024.

It supports:
- Simple and multi-hop queries
- Year-over-year comparisons
- Cross-company comparisons
- Segment-level breakdowns
- AI strategy analysis

The system uses a local semantic search engine built with Sentence Transformers and a Groq-hosted LLM for answer synthesis.

---

## Project Structure

```

Intern project/
└── src/
├── main.ipynb                      # Entry point to run the agent end-to-end
├── agent\_query\_engine.ipynb       # RAG agent logic with multi-step reasoning
├── embedder.ipynb                 # Embeds text chunks using sentence-transformers
├── chunker.ipynb                  # Splits long texts into chunks for retrieval
├── extractor.ipynb                # Extracts clean text from downloaded 10-K files
├── downloader.ipynb               # Optional: to download filings from EDGAR
├── requirements.txt               # Python dependencies
├── data/
│   ├── embedded\_chunks.json       # Final vector embeddings used by the agent
│   └── extracted/
│       └── NVDA\_2024\_10K.txt      # Example extracted raw 10-K file
├── chunks/
│   └── all\_chunks.json            # Text chunks before embedding
├── sample\_outputs/
└── ai\_investments\_comparison\_2024.json  # Sample output response

````

---

## Installation

### 1. Create a Virtual Environment (Recommended)

```bash
python -m venv venv
source venv/bin/activate         # macOS/Linux
venv\Scripts\activate            # Windows
````

### 2. Install Dependencies

```bash
pip install -r src/requirements.txt
```

---

## Setup Groq API Key

1. Go to [https://console.groq.com](https://console.groq.com)
2. Create an API key
3. In `agent_query_engine.ipynb`, paste your key here:

```python
client = Groq(api_key="your_actual_api_key_here")
```

> Do not commit your API key into version control.

---

## Step-by-Step Execution

You can run the entire project using Jupyter Notebooks. Open each notebook inside `src/` and run the cells in order:

### 1. Extract 10-K Text (Optional if already available)

Run `extractor.ipynb`

* Input: Raw 10-K files (PDF or text)
* Output: Cleaned plain text in `data/extracted/`

### 2. Chunk the Text

Run `chunker.ipynb`

* Input: Extracted 10-K text
* Output: Semantic chunks in `chunks/all_chunks.json`

### 3. Generate Embeddings

Run `embedder.ipynb`

* Input: Chunks from `chunks/all_chunks.json`
* Output: Embeddings in `data/embedded_chunks.json`

### 4. Run the Agent

Run `agent_query_engine.ipynb`

* Choose any query
* Agent will:

  * Decompose the query into sub-questions
  * Perform semantic search over embedded data
  * Use Groq's LLM to generate a detailed answer
  * Show reasoning steps, sub-queries, and cited sources

### 5. Or Run Main Pipeline

Run `main.ipynb` to test everything in one place.

---

## Supported Question Types

| Type           | Example                                                          |
| -------------- | ---------------------------------------------------------------- |
| Basic          | What was Microsoft's total revenue in 2023?                      |
| Year-over-Year | How did NVIDIA’s data center revenue grow from 2022 to 2023?     |
| Cross-Company  | Which company had the highest operating margin in 2023?          |
| Segment        | What percentage of Google’s revenue came from cloud in 2023?     |
| Strategy       | Compare AI investments mentioned by all companies in 2024 10-Ks. |

---

## Sample Output

```json
{
  "query": "Which company had the highest operating margin in 2023?",
  "answer": "Microsoft had the highest operating margin at 42.1%.",
  "sub_queries": [
    "Microsoft operating margin 2023",
    "Google operating margin 2023",
    "NVIDIA operating margin 2023"
  ],
  "reasoning": "Performed semantic search and comparison of 10-K filings.",
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

## Sample Queries to Try

```python
queries = [
    "What was Microsoft's total revenue in 2023?",
    "How did NVIDIA's data center revenue grow from 2022 to 2023?",
    "Which company had the highest operating margin in 2023?",
    "What percentage of Google's revenue came from cloud in 2023?",
    "Compare AI investments mentioned by all companies in 2024 10-Ks."
]
```

---

## Technologies Used

* **Python 3.8+**
* **SentenceTransformers** for embeddings
* **Groq LLM API** for query decomposition and answer generation
* **scikit-learn** for cosine similarity
* **Jupyter Notebook** for development and execution

---

## License

This project is provided for educational and demonstration purposes.

---

## Notes

* This implementation does not use a vector database like FAISS; everything is done in-memory.
* All embeddings are precomputed and stored in `data/embedded_chunks.json`.
* You may expand the system to include more companies or years by re-running the extract → chunk → embed pipeline.
