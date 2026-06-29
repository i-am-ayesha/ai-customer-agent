# 🤖 AI Customer Support Agent

> A production-ready intelligent support chatbot that answers customer questions directly from your company documentation — powered by RAG, LangChain, FAISS, Groq, and FastAPI.

---

## 📖 About

The AI Customer Support Agent is an intelligent chatbot system designed to automate customer support by retrieving accurate answers directly from your company's own documentation. Instead of relying on a generic AI that might hallucinate or give incorrect information, this system uses **Retrieval-Augmented Generation (RAG)** — meaning every answer is grounded in real documents you provide.

You upload your FAQs, product manuals, policy documents, or any text files, and the agent instantly becomes an expert on your business. Customers ask questions in natural language, and the agent finds the most relevant sections from your documents and generates a clear, accurate response using a powerful language model.

This project was built to solve a real problem faced by growing businesses: **the cost and slowness of human-only customer support**. By automating repetitive, document-based queries, support teams can focus on complex issues that truly need a human touch.

---

## 💡 Benefits

### For Businesses
- **Reduces support costs** by automating up to 75% of repetitive customer queries
- **Available 24/7** — no shifts, no holidays, no wait times
- **Consistent answers** — every customer gets the same accurate response from your official documentation
- **Instant scaling** — handles hundreds of simultaneous conversations without hiring more staff
- **Easy to update** — upload a new document and the agent immediately knows the updated information

### For Customers
- **Instant responses** — no more waiting in a queue for simple questions
- **Accurate answers** — responses come directly from official company documents, not guesswork
- **Natural conversation** — ask questions in plain English, just like talking to a person
- **Always available** — get help at 2 AM just as easily as during business hours

### For Developers
- **Fully open source stack** — no vendor lock-in, everything is customizable
- **Free to run** — uses Groq's free API tier and local HuggingFace embeddings, zero ongoing cost
- **Easy to extend** — clean, modular codebase makes it simple to add new features
- **REST API included** — integrate with any existing system via simple HTTP calls

---

## ✨ Features

- 🧠 **RAG Pipeline** — retrieves the most relevant document chunks before generating any answer, eliminating hallucinations
- 📄 **PDF & Text Upload** — drag and drop your company documents directly in the browser; supports `.pdf`, `.txt`, and `.md` files
- 💬 **Conversational Memory** — remembers the last 5 exchanges so follow-up questions work naturally
- ⚡ **Real-time Responses** — Groq's ultra-fast inference delivers answers in under a second
- 🎨 **Cosmic Glassmorphism UI** — stunning dark purple interface with nebula glow, 3D hover effects, and animated particles
- 📊 **Source Attribution** — every answer shows which document file it came from so customers can verify
- 🔍 **Smart Retrieval** — uses Maximal Marginal Relevance (MMR) to fetch diverse, relevant chunks instead of repetitive ones
- 🔄 **Multi-turn Dialogue** — maintains conversation context across multiple messages
- 🚀 **One-command startup** — single `uvicorn` command to launch the entire system
- 🐳 **Docker ready** — ship to production with a single `docker-compose up`

---

## 🛠️ Tech Stack

| Layer | Technology | Why It Was Chosen |
|---|---|---|
| **LLM** | Groq — Llama 3.3 70B | Free API, extremely fast inference, near GPT-4 quality |
| **Embeddings** | HuggingFace all-MiniLM-L6-v2 | Runs locally for free, no API cost, strong semantic understanding |
| **RAG Orchestration** | LangChain | Industry standard for building RAG pipelines, great abstractions |
| **Vector Store** | FAISS CPU | Facebook's battle-tested similarity search, fast and lightweight |
| **PDF Parsing** | PyMuPDF (fitz) | Reliable, fast PDF text extraction including complex layouts |
| **Backend** | FastAPI + Uvicorn | Modern async Python framework, automatic API docs, high performance |
| **Frontend** | Vanilla HTML/CSS/JS | Zero dependencies, instant load, fully customizable |
| **Container** | Docker + Docker Compose | Reproducible deployments anywhere |

---

## ⚔️ Challenges Faced

### 1. Python Version Compatibility
**Problem:** The project was initially tested on Python 3.14 (bleeding edge), which caused `tiktoken` to fail with Rust compilation errors since pre-built wheels weren't available yet.

**Solution:** Pinned the project to Python 3.9 which has full package support across the entire AI/ML ecosystem.

---

### 2. FAISS Version Mismatch
**Problem:** The original `requirements.txt` specified `faiss-cpu==1.8.0` which does not exist as a published package. Pip threw a resolution error.

**Solution:** Identified the correct available version `faiss-cpu==1.13.0` for Python 3.9 on Windows and updated the install command accordingly.

---

### 3. LangChain Dependency Conflicts
**Problem:** Installing `langchain-groq` pulled in `langchain-core==0.3.x` which was incompatible with `langchain==0.2.5`, causing a cascade of version conflicts across `langsmith`, `langchain-community`, and `langchain-openai`.

**Solution:** Removed all pinned version numbers and ran a single combined `pip install` command, letting pip's dependency resolver pick compatible versions automatically.

---

### 4. Garbage Output from Fake Embeddings
**Problem:** When switching to Groq without an OpenAI key, `FakeEmbeddings` were used as a placeholder. These generate random vectors, causing FAISS to retrieve completely wrong chunks and the LLM to produce garbled repetitive output like `+Rl J"FXB$Ads +Rl J"FXB$Ads...`.

**Solution:** Replaced `FakeEmbeddings` with `HuggingFaceEmbeddings` using the `all-MiniLM-L6-v2` model which runs entirely locally and produces real semantic vectors at zero cost.

---

### 5. PDF Binary Corruption
**Problem:** When users uploaded PDF files, the app was reading them as raw bytes and decoding with UTF-8, resulting in binary garbage being indexed into FAISS — making the RAG completely useless for PDF content.

**Solution:** Integrated `PyMuPDF (fitz)` to properly extract clean text from PDFs page by page before indexing.

---

### 6. .env File Not Loading
**Problem:** The `GROQ_API_KEY` was set in `.env` but the app kept running in demo mode. The issue was that Notepad on Windows saved the file as `.env.txt` instead of `.env`, and `load_dotenv()` was not being called before the RAG engine initialized.

**Solution:** Used the terminal command `echo GROQ_API_KEY=... > .env` to create the file correctly, and added `load_dotenv()` at the top of both `main.py` and `rag_engine.py`.

---

### 7. Groq Model Decommissioned
**Problem:** The original model `llama3-70b-8192` was retired by Groq mid-development, causing a `model_decommissioned` API error.

**Solution:** Updated the model name to `llama-3.3-70b-versatile`, Groq's current recommended replacement which is also more capable.

---

## 🚀 How to Use This Project

### Prerequisites
- Windows 10 or later
- Python 3.9 installed (`py -3.9 --version` should work)
- Internet connection (for Groq API calls)

---

### Step 1 — Extract the Project

Unzip `ai-customer-support.zip` into a folder:
```
C:\Users\YourName\Downloads\project\ai-customer-support
```

---

### Step 2 — Open Command Prompt

Navigate to the project folder:
```cmd
cd C:\Users\YourName\Downloads\project\ai-customer-support
```

---

### Step 3 — Create a Virtual Environment

```cmd
py -3.9 -m venv venv
venv\Scripts\activate
```

You will see `(venv)` at the start of your terminal line.

---

### Step 4 — Install Dependencies

```cmd
pip install fastapi uvicorn[standard] python-multipart pydantic langchain langchain-openai langchain-community langchain-groq langchain-core openai python-dotenv tiktoken faiss-cpu==1.13.0 groq sentence-transformers pymupdf
```

This may take 3–5 minutes as it downloads the embedding model.

---

### Step 5 — Get a Free Groq API Key

1. Go to **console.groq.com**
2. Sign up — free, no credit card required
3. Click **API Keys → Create API Key**
4. Copy the key (starts with `gsk_...`)

---

### Step 6 — Configure Your API Key

```cmd
echo GROQ_API_KEY=gsk_your_key_here > .env
```

Verify it saved correctly:
```cmd
type .env
```

---

### Step 7 — Start the Server

```cmd
uvicorn app.main:app --reload
```

Wait until you see:
```
✅ Groq API key found — using Llama3 via Groq
✅ Loaded sample docs: 11 chunks indexed
INFO:     Application startup complete.
```

---

### Step 8 — Open in Your Browser

Go to **http://localhost:8000**

---

### Step 9 — Upload Your Documents

1. Click **"Upload Docs"** in the top right corner
2. Select any `.pdf`, `.txt`, or `.md` file (your FAQs, policies, manuals)
3. Wait for the success message
4. Start asking questions about your document

---

### Step 10 — Start Chatting

Type any question in the input box and press **Enter**. The agent will:
1. Search your uploaded documents for relevant sections
2. Generate a clear answer based only on what's in your documents
3. Show you which file the answer came from

---

### Stopping the Server
```
Ctrl + C
```

### Starting Again Next Time
```cmd
cd C:\Users\YourName\Downloads\project\ai-customer-support
venv\Scripts\activate
uvicorn app.main:app --reload
```

---

## 📝 License

MIT — free to use, modify, and distribute.
