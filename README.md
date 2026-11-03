# PDF RAG Chatbot

A Streamlit application for asking questions about an uploaded PDF. It extracts document text, splits it into overlapping chunks, creates sentence-transformer embeddings, retrieves relevant chunks with FAISS, and uses Groq-hosted Llama 3.1 to generate conversational answers.

## How it works

```text
PDF upload → PyPDFLoader → Recursive text splitting
(chunk size: 1,000; overlap: 200)
→ Sentence Transformers (all-MiniLM-L6-v2)
→ FAISS retrieval (top 3 chunks)
→ Conversational Retrieval Chain (Groq)
→ Streamlit answer
```

## Features

- Upload and process a PDF in the Streamlit interface.
- Ask natural-language questions about the document.
- Retrieve relevant text chunks using FAISS.
- Maintain conversational context during the active Streamlit session.
- Use Groq for LLM inference and a local Sentence Transformers embedding model.

## Technology

- Python and Streamlit
- LangChain and LangChain Community
- Groq (llama-3.1-8b-instant)
- Sentence Transformers (all-MiniLM-L6-v2)
- FAISS CPU and PyPDF

## Requirements

- Python 3.10 or later
- A Groq API key
- Internet access for Groq inference and the first embedding-model download

## Setup

1. Clone:

```bash
git clone https://github.com/M-Abdullah-Shahzad/rag-pdf-chatbot.git
cd rag-pdf-chatbot
```

2. Create and activate a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

3. Install dependencies:

```bash
python -m pip install -r requirements.txt
```

4. Create a local .env file containing:

```env
GROQ_API_KEY=your_groq_api_key_here
```

5. Start the app:

```bash
streamlit run app.py
```

## Current implementation notes

- A FAISS index is built from the currently uploaded PDF; a persistent document library is not implemented.
- Retrieval returns up to three chunks per question.
- The current interface displays answers but does not render source citations or page references.
- The upload is written to a local file named uploaded.pdf; concurrent users on a shared deployment may overwrite it. Use per-session temporary storage before multi-user deployment.
- The project currently uses LangChain ConversationalRetrievalChain and ConversationBufferMemory APIs. Pin dependencies before relying on reproducible deployments.

## Security and privacy

- Keep .env out of Git and never commit API keys.
- Do not upload confidential documents to a hosted deployment unless you are authorized to share them with the configured LLM provider.
- Check generated answers against the original PDF.

## License

No license is currently declared. Contact the repository owner before redistributing this project.