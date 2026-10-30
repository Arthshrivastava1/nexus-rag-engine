# Nexus RAG Engine

Nexus is a document based conversation workspace for finding and discussing internal knowledge. Teams can use it to search HR policies, IT runbooks, finance procedures, stakeholder agreements, and other company documents, then review answers against the retrieved source material.

## Capabilities

- Upload and query PDF, DOCX, DOC, TXT, and Markdown documents (the application upload route currently also accepts CSV).
- Hybrid lexical and semantic retrieval with document chunking and reranking.
- Shared and personal document collections, with user and administrator workflows.
- A FastAPI backend that also serves the bundled frontend and its local images.

## Run locally

Install Python dependencies and start the API from the backend directory:

```bash
cd backend
pip install -r requirements.txt
python -m uvicorn main:app --reload --port 8000
```

Open [http://localhost:8000/](http://localhost:8000/) for the website. The frontend pages and files in `frontend/images/` are served by the same backend, so deployed pages do not depend on third party image URLs.

## Configuration

Set configuration in the deployment environment rather than committing secrets:

| Variable | Purpose |
| --- | --- |
| `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, `DB_NAME` | MySQL connection |
| `JWT_SECRET` or `APP_SECRET_KEY` | Signing authentication tokens; set a long random value |
| `GOOGLE_API_KEY` | Gemini response generation |
| `GEMINI_MODEL` | Optional model override |
| `COHERE_API_KEY` | Optional reranking service; lexical fallback is used if absent |
| `USE_LOCAL_EMBEDDINGS` | Set to `true` to use local embeddings |

Ensure the database is reachable from the application host and that Chroma storage is writable by the process. For production, configure TLS at the hosting layer, use managed secrets and persistent storage, and keep credentials out of source control.