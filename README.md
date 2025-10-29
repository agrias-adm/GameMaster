# Game Master

A small RAG (Retrieval-Augmented Generation) utility to index PDF game manuals/guides and build a searchable vector database. Intended use: give the project a game manual or guide (PDFs in `data/`), run the importer to create a Chroma vector store, then query that store from a separate QA application to ask questions about the game (acts like a game master / rules assistant).

## What it does
- Loads all PDFs from the `data/` folder.
- Splits each PDF into text chunks.
- Generates embeddings (via `get_embedding_function.py`) and stores vectors in a persistent Chroma DB (`chroma/`).
- Skips chunks that are already present (by computed IDs).
- Shows progress via `tqdm`.

## Inputs
- PDF files placed in `data/` (one or more PDFs).

## Outputs
- Persistent Chroma vector database in the `chroma/` directory.
- Console logs showing progress and counts.

## Quick start

1. Create `data/` and put your game manual PDFs there.
2. Install dependencies (example using `uv` wrapper):
   ```
   uv pip install langchain langchain-text-splitters langchain-community chromadb tqdm pypdf
   ```
3. Run the importer:
   ```
   uv python main.py
   ```
   To clear the existing DB and rebuild:
   ```
   uv python main.py --reset
   ```

## Notes
- `get_embedding_function.py` currently returns `OllamaEmbeddings(model="nomic-embed-text:latest")`. You need an Ollama server/local model or replace this with another embeddings provider (OpenAI, HuggingFace, etc.).
- This repository only builds the vector store. To ask questions you need a separate QA / RAG script that loads `Chroma(persist_directory="chroma", embedding_function=...)` and runs similarity search + a generation model.
- Progress bar (tqdm) shows chunk addition progress; console logs show counts of documents and added chunks.

## Extending
- Add a QA CLI or API that loads the DB and answers queries.
- Swap embeddings provider in `get_embedding_function.py` if needed.
- Add timing/logging code to estimate runtime & data throughput (optional).

## License
Use as needed; add a license file if required.