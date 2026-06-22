# 🎬 AI Video Assistant

An AI-powered meeting/video intelligence tool that transcribes, summarises, and lets you **chat with any video or meeting** using Retrieval-Augmented Generation (RAG).

Paste a YouTube link or upload a local audio/video file, and the app will:
- Transcribe the audio (English via **Whisper**, Hindi/Hinglish via **Sarvam AI**)
- Generate a professional title and summary
- Extract action items, key decisions, and open questions
- Let you ask follow-up questions about the content via a RAG-powered chat interface

---

## ✨ Features

- 🎙️ **Dual transcription engines** — OpenAI Whisper (English) + Sarvam AI Speech-to-Text-Translate (Hindi/Hinglish)
- 📝 **Automatic summarisation** — map-reduce summarisation pipeline using Mistral AI
- ✅ **Action item extraction** — automatically pulls out tasks, owners, and deadlines
- 🔑 **Key decision & open question detection**
- 💬 **RAG-powered chat** — ask questions about the transcript, answered using only the video's content (ChromaDB + LangChain)
- 🎨 **Premium dark UI** — custom-styled Streamlit interface

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend / UI | Streamlit |
| LLM | Mistral AI (`mistral-small-latest`) via LangChain |
| Speech-to-Text | OpenAI Whisper (local) + Sarvam AI (API) |
| Orchestration | LangChain (LCEL) |
| Vector Store | ChromaDB |
| Embeddings | HuggingFace `sentence-transformers` (`all-MiniLM-L6-v2`) |
| Audio/Video Processing | yt-dlp, pydub, moviepy, ffmpeg |
| Language | Python 3.12 |

---

## 📂 Project Structure

AI-Video-Assistant/

├── core/

│   ├── transcriber.py      # Whisper + Sarvam AI transcription

│   ├── summarizer.py       # Map-reduce summary + title generation

│   ├── extractor.py        # Action items / decisions / questions

│   ├── vector_store.py     # ChromaDB embedding + retrieval

│   └── rag_engine.py       # LangChain LCEL RAG pipeline

├── utils/

│   └── audio_processor.py  # YouTube download, audio conversion, chunking

├── app.py                  # Streamlit UI

├── main.py                 # CLI entry point

├── test.py                 # Quick pipeline smoke test

├── requirements.txt

└── runtime.txt             # Pins Python version for deployment

---

## ⚙️ Setup & Local Installation

### 1. Clone the repository
```bash
git clone https://github.com/mhd-faraz/AI-Video-Assistant.git
cd AI-Video-Assistant
```

### 2. Create a virtual environment (Python 3.12 recommended)
```bash
python3 -m venv venv
source venv/bin/activate      # On Windows: venv\Scripts\activate
```

### 3. Install dependencies
```bash
pip install --upgrade pip
pip install "setuptools<81"
pip install -r requirements.txt
```

### 4. Install ffmpeg (required for audio processing)
```bash
# macOS
brew install ffmpeg

# Ubuntu/Debian
sudo apt install ffmpeg
```

### 5. Set up environment variables
Create a `.env` file in the root directory:
MISTRAL_API_KEY=your_mistral_api_key

SARVAM_API_KEY=your_sarvam_api_key

HF_TOKEN=your_huggingface_token

WHISPER_MODEL=small

SARVAM_STT_MODEL=saaras:v2.5

### 6. Run the app
```bash
streamlit run app.py
```

---

## 🔑 Getting API Keys

- **Mistral AI:** [console.mistral.ai](https://console.mistral.ai) → API Keys
- **Sarvam AI:** [dashboard.sarvam.ai](https://dashboard.sarvam.ai) → API Keys
- **HuggingFace:** [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens) → New token (Read access)

---

## 🚀 Deployment

This app is configured for deployment on **Render**, with `runtime.txt` pinning the Python version to avoid dependency issues (e.g. the `pyaudioop`/`audioop` removal in Python 3.13, which breaks `pydub`).

---

## 📌 Notes

- Language selector in the UI: `english` routes to Whisper (local), `hinglish` routes to Sarvam AI (API, translates to English while transcribing).
- Sarvam's synchronous API only accepts audio clips up to 30 seconds, so longer chunks are automatically split before being sent.

---

## 📄 License

This project is for educational and portfolio purposes.