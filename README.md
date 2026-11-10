# txtai RAG App

Semantic search + Retrieval-Augmented Generation with citations, built on
[txtai](https://github.com/neuml/txtai) — the open-source embeddings database
that combines vector search, graphs, and SQL in one store.

This is the Python code behind the txtai RAG space app. It mirrors the app's
tabs in a command-line program:

| App tab | What it does here |
| --- | --- |
| Library | Load `.txt` / `.md` files, chunk them, index them |
| Search | Raw similarity search with scores (`Embeddings.search`) |
| Ask | RAG question-answering grounded in retrieved context, with a citation to the source chunk |
| About | See the docstring at the top of `txtai_rag_app.py` |

## Quickstart

```bash
pip install -r requirements.txt
python txtai_rag_app.py --demo --ask "What was won?"
```

The demo uses the sample corpus from the official txtai docs. The first run
downloads the embedding model (`all-MiniLM-L6-v2`) and the LLM
(`Qwen/Qwen3-0.6B`) from Hugging Face.

## Usage

```bash
# Your own documents: index every .txt / .md file in a folder
python txtai_rag_app.py --data ./docs --ask "What does the refund policy say?"
python txtai_rag_app.py --data ./docs --search "refund deadline"

# Interactive mode (questions, or 'search: ...' for raw search)
python txtai_rag_app.py --data ./docs --interactive

# Save / reuse an index instead of re-indexing every run
python txtai_rag_app.py --data ./docs --save-index ./index
python txtai_rag_app.py --load-index ./index --ask "What was won?"

# Use a different LLM (any Hugging Face / llama.cpp / LiteLLM path txtai supports)
python txtai_rag_app.py --demo --model Qwen/Qwen3-4B --ask "What was won?"
```

## Testing

Every push runs the demo end to end on GitHub Actions
(`.github/workflows/test.yml`): it installs the dependencies and asks the
docs' own question, "What was won?", checking the answer mentions the
lottery win.

To try it interactively in the browser, open the repo in
[GitHub Codespaces](https://docs.github.com/en/codespaces) and run the
quickstart commands above.

## How it differs from the hosted demo

The hosted space app ranks chunks with TF-IDF similarity. This code uses
real txtai: neural embeddings for ranking and a local LLM for generation.
