# ✨ SparkAI — LLM-Powered Multimodal AI Assistant

SparkAI is an **LLM-powered multimodal AI assistant** built with **Python**, **Streamlit**, and **LangChain**. It combines general AI conversations with a **Retrieval-Augmented Generation (RAG)** pipeline for document-based question answering, along with OCR-powered processing for scanned documents and support for image-based interactions.

---

## 🚀 Key Features

### 💬 General AI Chat

* **Interactive AI Chat:** Engage in natural conversations through a clean, Gemini-inspired conversational interface.
* **Multiple LLM Providers:** Integrates **Google Gemini** and **Groq** APIs for AI-powered text generation.
* **Real-Time Streaming:** Displays AI responses progressively for a responsive conversational experience.
* **Context-Aware Conversations:** Maintains recent conversation history to provide more relevant responses.
* **Persistent Chat Sessions:** Manage multiple conversations within the application session.
* **Chat Management:** Create, rename, pin, delete, search, and branch conversations.
* **Message Editing:** Edit previous prompts and regenerate responses.
* **Response Regeneration:** Regenerate assistant responses when needed.
* **Transcript Exporting:** Export conversations for later use or documentation.
* **Text-to-Speech:** Listen to assistant responses using browser-based speech synthesis.

### 📚 Retrieval-Augmented Generation (RAG)

* **Document Question Answering:** Ask questions based on information contained in uploaded documents.
* **Multi-Format Document Support:** Process PDF, DOCX, TXT, and Markdown-based document content where supported.
* **Text Chunking:** Splits extracted document content into smaller chunks for efficient retrieval.
* **Embedding Generation:** Generates semantic embeddings using **Hugging Face Sentence Transformers**.
* **Vector Search:** Uses **FAISS** for semantic similarity search and document retrieval.
* **Context-Aware Responses:** Relevant document content is supplied to the LLM to generate document-aware answers.
* **Incremental Indexing:** Adds newly uploaded document chunks without unnecessarily re-processing previously indexed content.
* **Document Hashing:** Identifies previously processed files and reduces redundant embedding generation.
* **Persistent Vector Indexing:** Stores FAISS indexes locally for reuse.

### 📄 OCR & Document Processing

* **Scanned PDF Support:** Detects pages with insufficient native text extraction and applies OCR as a fallback.
* **Page-Level OCR:** OCR is applied only to pages that require it.
* **Scanned Document Support:** Extracts readable text from scanned documents.
* **Handwritten Document Support:** Attempts OCR extraction from handwritten pages using Tesseract.
* **OCR Fallback:** Uses native PDF extraction first and falls back to Tesseract when required.
* **Duplicate Chunk Removal:** Removes duplicate extracted chunks before embedding.

> **Note:** Tesseract OCR works best with printed or typed text. Handwritten or highly stylized documents may produce imperfect OCR results.

### 🖼️ Image Understanding

* **Image Input:** Upload images as part of a conversation.
* **Multimodal Processing:** Uses Google Gemini's multimodal capabilities to analyze uploaded images together with user questions.
* **Image-Based Questions:** Ask questions about the content of uploaded images.

### 🎨 Experimental Image Generation

* **Text-to-Image Capability:** SparkAI includes an experimental text-to-image feature that accepts natural-language prompts.
* **External Generation Service:** Image generation is handled through an external image-generation service.
* **Experimental Quality:** Generated results may vary depending on the prompt and external generation service.

> Image generation is currently treated as an experimental capability rather than a core document or conversational feature.

---

## 🛠️ Tech Stack

### Frontend & UI
* **Streamlit** — Interactive web interface and conversational UI
* **HTML / CSS** — Custom interface styling
* **JavaScript** — Client-side interactions and browser features

### Application & AI
* **Python** — Core application logic
* **LangChain** — LLM and RAG integration
* **Google Gemini API** — General and multimodal AI responses
* **Groq API** — Fast LLM inference

### RAG & Semantic Search
* **FAISS** — Vector storage and semantic similarity search
* **Hugging Face Sentence Transformers** — Document embedding generation
* **Incremental Indexing** — Avoids unnecessary document re-processing
* **Document Hashing** — Detects previously processed files
* **Batch Embedding** — Processes document chunks efficiently

### Document Processing
* **PyPDF / PyMuPDF** — PDF processing
* **python-docx** — DOCX document processing
* **Tesseract / pytesseract** — OCR for scanned documents
* **Pillow** — Image processing
* **Markdown** — Markdown content handling

### Deployment
* **Docker** — Application containerization
* **Streamlit Community Cloud** — Application serving and deployment

---

## 🏗️ Application Architecture

SparkAI uses a modular, multi-path architecture that handles conversational chat, multimodal image inputs, and document-based Retrieval-Augmented Generation (RAG):

```text
                        ┌─────────────────────┐
                        │        User         │
                        └──────────┬──────────┘
                                   │
                                   ▼
                        ┌─────────────────────┐
                        │    Streamlit UI     │
                        │  Chat + File Input  │
                        └──────────┬──────────┘
                                   │
                ┌──────────────────┼──────────────────┐
                │                  │                  │
                ▼                  ▼                  ▼
        ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
        │ General AI   │    │ Document RAG │    │    Images    │
        │    Chat      │    │   Pipeline   │    │ Understanding│
        └──────┬───────┘    └──────┬───────┘    └──────┬───────┘
               │                   │                   │
               │                   ▼                   │
               │          ┌────────────────┐           │
               │          │    Document    │           │
               │          │    Loading     │           │
               │          └───────┬────────┘           │
               │                  │                    │
               │                  ▼                    │
               │          ┌────────────────┐           │
               │          │  Text / OCR    │           │
               │          │  Extraction    │           │
               │          └───────┬────────┘           │
               │                  │                    │
               │                  ▼                    │
               │          ┌────────────────┐           │
               │          │ Text Chunking  │           │
               │          └───────┬────────┘           │
               │                  │                    │
               │                  ▼                    │
               │          ┌────────────────┐           │
               │          │   Sentence     │           │
               │          │  Transformers  │           │
               │          └───────┬────────┘           │
               │                  │                    │
               │                  ▼                    │
               │          ┌────────────────┐           │
               │          │     FAISS      │           │
               │          │  Vector Store  │           │
               │          └───────┬────────┘           │
               │                  │                    │
               │                  ▼                    │
               │          ┌────────────────┐           │
               │          │    Semantic    │           │
               │          │   Retrieval    │           │
               │          └───────┬────────┘           │
               │                  │                    │
               └──────────────────┼────────────────────┘
                                  │
                                  ▼
                        ┌─────────────────────┐
                        │ LangChain / Prompt  │
                        │    Orchestration    │
                        └──────────┬──────────┘
                                   │
                        ┌──────────┴──────────┐
                        │                     │
                        ▼                     ▼
                ┌─────────────┐       ┌─────────────┐
                │    Groq     │       │   Gemini    │
                │     API     │       │     API     │
                └──────┬──────┘       └──────┬──────┘
                       │                     │
                       └──────────┬──────────┘
                                  │
                                  ▼
                        ┌─────────────────────┐
                        │     AI Response     │
                        └─────────────────────┘
```

---

## 🔄 RAG Pipeline Details

The document processing pipeline implements dynamic fallback logic to extract and structure data from both raw text files and scanned documents seamlessly:

```text
Uploaded Document
        │
        ▼
Document Format Router (PDF / DOCX / TXT / MD)
        │
        ▼
Text Extraction
        │
        ▼
Is Extracted Text Sufficient?
  ├── Yes ──> Proceed to Chunking
  └── No  ──> Tesseract OCR Engine (Fallback) ──> Proceed to Chunking
        │
        ▼
Recursive Character Text Chunking
        │
        ▼
Hugging Face Sentence Transformers (Embeddings)
        │
        ▼
FAISS Vector Store Indexing
        │
        ▼
Similarity Vector Search (Top-k Context Retrieval)
        │
        ▼
Prompt Augmentation & Context Injected into LLM
        │
        ▼
Streamed Response Generation
```

* **Dynamic OCR Fallback:** Automatically evaluates page text density to trigger OCR only on scanned or low-text PDF pages.
* **Semantic Embeddings:** Uses Hugging Face Sentence Transformers to map chunked document content into a high-dimensional vector space.
* **Vector Indexing:** FAISS indexes embeddings locally for near-instant similarity searches during user queries.

---

## ⚡ Performance Optimizations

SparkAI implements several optimizations to reduce unnecessary processing during document ingestion:

* **Incremental Indexing:** Only newly uploaded content is processed when adding documents to an existing vector store.
* **Document Hashing:** Previously processed documents are identified without re-processing them.
* **Batch Embedding:** Embeddings are generated in batches to better control CPU and memory usage.
* **Persistent FAISS Indexes:** Vector indexes can be saved and reused.
* **Embedding Model Caching:** The embedding model is cached during the application lifecycle.
* **Page-Level OCR Fallback:** OCR is applied only to PDF pages where native extraction produces insufficient content.
* **Duplicate Chunk Removal:** Repeated chunks are removed before indexing.

---

## 📁 Project Structure

```text
SparkAI/
├── .devcontainer/           # Development container configuration
├── .streamlit/
│   └── config.toml          # Streamlit theme and server configuration
├── .dockerignore            # Docker build exclusions
├── .gitignore               # Git ignored files
├── Dockerfile               # Docker container configuration
├── README.md                # Project documentation
├── app.py                   # Main Streamlit application
├── doc_loader.py            # DOCX and TXT document loading
├── llm.py                   # LLM and AI generation integrations
├── pdf_loader.py            # PDF extraction and OCR processing
├── requirements.txt         # Python dependencies
├── text_splitter.py         # Document chunking and duplicate removal
└── vector_store.py          # FAISS embeddings, indexing and retrieval
```

---

## ⚙️ Getting Started

### Prerequisites
* Python 3.10 or 3.11
* Git
* Tesseract OCR
* Poppler
* Google Gemini API Key
* Groq API Key
* Docker (optional)

### System Dependencies Installation

**Ubuntu / Debian:**
```bash
sudo apt update
sudo apt install tesseract-ocr poppler-utils
```

**macOS:**
```bash
brew install tesseract poppler
```

---

## 📥 Installation

### 1. Clone the Repository
```bash
git clone [https://github.com/rajesh33399/SparkAI.git](https://github.com/rajesh33399/SparkAI.git)
cd SparkAI
```

### 2. Create a Virtual Environment
```bash
python -m venv venv
```

### 3. Activate the Virtual Environment
* **Linux / macOS:**
  ```bash
  source venv/bin/activate
  ```
* **Windows:**
  ```cmd
  venv\Scripts\activate
  ```

### 4. Install Dependencies
```bash
pip install -r requirements.txt
```

### 5. Configure API Keys
Set your API keys using environment variables or `.streamlit/secrets.toml`:

```env
GROQ_API_KEY="your_groq_api_key"
GEMINI_API_KEY="your_gemini_api_key"
```

> **Warning:** Never commit API keys, tokens, or secret configuration files to GitHub.

### 6. Run SparkAI
```bash
streamlit run app.py
```
Access the application locally at `http://localhost:8501`.

---

## 🐳 Running with Docker

1. **Build the Docker image:**
   ```bash
   docker build -t sparkai .
   ```

2. **Run the container:**
   ```bash
   docker run -p 8501:8501 \
     -e GROQ_API_KEY="your_groq_api_key" \
     -e GEMINI_API_KEY="your_gemini_api_key" \
     sparkai
   ```

---

## 💡 Usage

* **General AI Chat:** Open SparkAI, start a chat, and interact with the streaming LLM interface.
* **Document Question Answering:** Upload PDF, DOCX, TXT, or MD files. The app chunks, embeds, and indexes content into FAISS to enable context-aware document Q&A.
* **Image Understanding:** Attach images directly into the chat and prompt Gemini's vision model to extract details or describe visual elements.
* **Experimental Image Generation:** Open the image generation tab and supply natural-language prompts to generate visual outputs.

---

## 📄 Supported Document Types

* `.pdf`
* `.docx`
* `.txt`
* `.md`

---

## 🔐 Security

* API credentials are loaded dynamically through environment variables or Streamlit secrets.
* Confidential configuration files (`.env`, `secrets.toml`) are explicitly excluded from Git via `.gitignore`.

---

## ⚠️ Limitations

* Response latency and output quality depend directly on selected LLM models and API tier limits.
* OCR extraction accuracy on handwritten text varies depending on scan clarity and document structure.
* Processing extensive scanned PDFs requires additional time due to CPU-bound OCR and embedding operations.

---

## 🚧 Future Improvements

* 🎬 **Video Generation:** Expand multimodal support to include text-to-video capabilities.
* 🔎 **Hybrid Search:** Integrate sparse BM25 keyword search alongside dense FAISS vector retrieval.
* 📊 **Structured Data Parsing:** Enable native tabular analysis for `.csv` and `.xlsx` files.
* 🌐 **Web Retrieval:** Allow real-time web page indexing and content retrieval.
* 🎙️ **Voice Capabilities:** Implement real-time speech synthesis and audio input options.

---

## 📌 Project Status

SparkAI is actively maintained as a full-stack LLM demonstration showcase integrating RAG, computer vision, and streaming LLM architectures in Python.

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for details.
``░
