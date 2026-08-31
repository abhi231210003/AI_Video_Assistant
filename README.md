# 🎙️ AI Video & Meeting Assistant

An AI-powered video and meeting intelligence platform built with **Python**, **OpenAI Whisper**, **Sarvam AI**, **LangChain (LCEL)**, **Mistral AI**, **ChromaDB**, and **Streamlit**.

Transform any YouTube video, recorded meeting, lecture, or local audio/video file into structured transcripts, executive summaries, actionable insights, and an interactive **RAG (Retrieval-Augmented Generation) Chatbot**.

---

## 🌟 Key Features

- 📥 **Versatile Media Ingestion**:
  - Download audio directly from any YouTube URL using `yt-dlp`.
  - Process local video and audio formats (`.mp4`, `.mkv`, `.mp3`, `.wav`, `.m4a`, etc.).
  - Automatic conversion to 16kHz mono WAV format via `pydub` and `ffmpeg`.
  - Smart audio chunking (10-minute segments) for long-duration recordings.

- 🗣️ **Multilingual Speech-to-Text**:
  - **English**: High-precision local transcription using **OpenAI Whisper** (configurable models: `tiny`, `base`, `small`, `medium`, `large`).
  - **Hindi / Hinglish**: Indian language speech-to-text and translation powered by **Sarvam AI API** (`saaras:v3`).

- 🧠 **AI-Powered Meeting Insights (Mistral AI + LangChain)**:
  - **Automated Title Generation**: Creates clear, concise meeting titles.
  - **Executive Summary**: Generates structured, bulleted meeting summaries.
  - **Action Items**: Extracts actionable tasks, assigned owners, and deadlines.
  - **Key Decisions**: Pinpoints final consensus and critical decisions.
  - **Open Questions**: Highlights unresolved topics and follow-ups.

- 💬 **RAG Q&A Chat with Transcript**:
  - Text chunking with `RecursiveCharacterTextSplitter`.
  - Local vector database using **ChromaDB** and HuggingFace embeddings (`all-MiniLM-L6-v2`).
  - Full LangChain LCEL pipeline to chat interactively with the meeting transcript.

- 🎨 **Modern Streamlit Web UI**:
  - Dark-mode glassmorphic interface with custom typography (`Syne` & `JetBrains Mono`).
  - Real-time pipeline step progress tracker.
  - Interactive transcript viewer and chat bubble UI.
  - CLI fallback via `main.py`.

---

## 🏗️ Architecture & Pipeline Flow

```
[ YouTube URL / Local Audio / Video ]
                  │
                  ▼
         [ Audio Processor ]
    (yt-dlp ➔ pydub/ffmpeg conversion ➔ 10-min chunking)
                  │
                  ▼
        [ Transcriber Engine ]
    ┌─────────────────────────┐
    │ English  ➔ OpenAI Whisper │
    │ Hinglish ➔ Sarvam AI STT │
    └─────────────────────────┘
                  │
                  ▼
          [ Full Transcript ]
                  │
  ┌───────────────┼──────────────────────────────┐
  ▼               ▼                              ▼
[ Title ]    [ Summarizer ]               [ Extraction Engine ]
(Mistral)      (Mistral)             (Action Items / Decisions / Qs)
  │               │                              │
  └───────────────┼──────────────────────────────┘
                  ▼
          [ RAG Vector Store ]
(ChromaDB + HuggingFace all-MiniLM-L6-v2)
                  │
                  ▼
       [ Interactive RAG Chat ]
    (ChatMistralAI + LCEL Pipeline)
```

---

## 📁 Project Structure

```text
aivideoassistant/
├── core/
│   ├── transcriber.py       # Whisper and Sarvam AI STT logic
│   ├── summarizer.py        # Title generation and executive summaries
│   ├── extractor.py         # Action items, decisions, and open questions extraction
│   ├── vector_store.py      # ChromaDB client, HuggingFace embeddings, and vector indexing
│   └── rag_engine.py        # LangChain LCEL RAG chain and QA retrieval
├── utils/
│   └── audio_processor.py   # YouTube downloading, format conversion, and audio chunking
├── downloads/               # Temporary storage for downloaded audio files
├── vector_db/               # Persistent ChromaDB vector store
├── .env                     # API keys and environment configuration
├── app.py                   # Streamlit web application
├── main.py                  # CLI pipeline entry point
├── Requirements.txt         # Project dependencies
└── README.md                # Documentation
```

---

## 🚀 Getting Started

### 1. Prerequisites
- **Python >= 3.10**
- **[FFmpeg](https://ffmpeg.org/)** installed and added to your system `PATH`.
- **[uv](https://github.com/astral-sh/uv)** (recommended for fast package management) or standard `pip`.

### 2. Clone the Repository & Navigate
```bash
git clone <repo-url>
cd aivideoassistant
```

### 3. Set Up Virtual Environment & Dependencies
Using `uv` (recommended):
```powershell
uv venv
uv pip install -r Requirements.txt
```

Or using standard `pip`:
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r Requirements.txt
```

### 4. Configure Environment Variables
Create a `.env` file in the root directory:
```env
# Required for Summaries, Extractions & RAG Chat
MISTRAL_API_KEY=your_mistral_api_key_here

# Required for Hindi/Hinglish Transcription (Optional if only English is needed)
SARVAM_API_KEY=your_sarvam_api_key_here

# Whisper Model (tiny, base, small, medium, large) - Default: small
WHISPER_MODEL=small
```

---

## 💻 Running the Application

### Option A: Streamlit Web UI (Recommended)
Run the web application with `uv`:
```powershell
uv run python -m streamlit run app.py
```
Or after activating your `.venv`:
```powershell
streamlit run app.py
```
Open **http://localhost:8501** in your browser.

### Option B: Command-Line Interface (CLI)
Run the interactive CLI:
```powershell
uv run python main.py
```
Follow the terminal prompts to provide a YouTube URL or file path and chat directly in your terminal.

---

## 📦 Key Dependencies

| Component | Library |
| :--- | :--- |
| **STT Engine** | `openai-whisper`, `torch`, `torchaudio` |
| **Audio Processing** | `yt-dlp`, `pydub`, `ffmpeg-python` |
| **LLM & Orchestration** | `langchain`, `langchain-core`, `langchain-mistralai`, `mistralai` |
| **Embeddings & Vector Store** | `chromadb`, `langchain-chroma`, `sentence-transformers`, `langchain-huggingface` |
| **Web UI** | `streamlit`, `streamlit-extras` |

---

## 🛡️ License

This project is licensed under the MIT License.
