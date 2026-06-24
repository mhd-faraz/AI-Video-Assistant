# 🎬 AI Video Assistant

**Meeting Intelligence Platform** — Real-time transcription, AI-powered summarization, and intelligent Q&A from video/audio files.

![Status](https://img.shields.io/badge/status-fully%20functional%20locally-brightgreen?style=flat-square)
![Python](https://img.shields.io/badge/python-3.12-blue?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)

---

## ✨ Features

### 🎯 Core Capabilities
- **Real-time Transcription** — OpenAI Whisper with audio extraction from video
- **AI Summarization** — Mistral AI generates concise meeting summaries
- **RAG-based Q&A** — Ask questions about your meeting transcript using ChromaDB + LangChain
- **Hinglish Support** — Transcribe and chat in Hindi/English mix (Sarvam AI)
- **Multiple File Formats** — MP4, MOV, MP3, WAV, M4A, AVI, MKV

### 🎨 User Experience
- **Premium Dark UI** — Custom Streamlit theme with gradient text, smooth animations
- **Real-time Pipeline Status** — Live status indicators (🟢 processing, ✅ done)
- **Interactive Chat** — Have conversations with your meeting data
- **Transcript Export** — View full transcripts with expandable sections

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | Streamlit 1.38.0 |
| **Backend** | Python 3.12, LangChain 0.3.7 |
| **Transcription** | OpenAI Whisper 20240930 |
| **LLM** | Mistral AI (7B) via API |
| **Vector Store** | ChromaDB 0.5.18 |
| **Embeddings** | Sentence Transformers (all-MiniLM-L6-v2) |
| **Audio Processing** | FFmpeg, MoviePy, pydub |
| **Additional** | Sarvam AI (Hinglish STT) |

---

## 🚀 Quick Start

### Prerequisites
- **Python 3.12+** (3.13 not supported due to `pyaudioop` removal)
- **FFmpeg** (for audio/video processing)
- **API Keys** (Mistral, Sarvam AI, HuggingFace)

### Installation

```bash
# 1. Clone repository
git clone https://github.com/mhd-faraz/AI-Video-Assistant
cd AI-Video-Assistant-App

# 2. Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Set up environment variables
cp .env.example .env  # Or create .env manually
# Edit .env with your API keys (see below)

# 5. Run the app
streamlit run app.py
```

Browser opens at `http://localhost:8501`

---

## 🔑 API Keys Setup

Create a `.env` file in project root:

```env
# Mistral AI (for summarization)
MISTRAL_API_KEY=your_mistral_key_here

# Sarvam AI (for Hinglish transcription)
SARVAM_API_KEY=your_sarvam_key_here

# HuggingFace (for embeddings model)
HF_TOKEN=your_huggingface_token_here

# Model configurations
WHISPER_MODEL=small  # small, medium, large (larger = slower but more accurate)
SARVAM_STT_MODEL=saaras:v2.5
```

### Getting API Keys

1. **Mistral AI** → [console.mistral.ai](https://console.mistral.ai)
2. **Sarvam AI** → [sarvam.ai](https://www.sarvam.ai)
3. **HuggingFace** → [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)

---

## 📖 Usage

### 1. Upload Video/Audio File
- Sidebar → Select **"Upload File"**
- Drag & drop or browse: MP4, MOV, MP3, WAV, M4A, AVI, MKV
- Maximum: **500 MB locally**

### 2. Choose Language
- **English** — Standard English transcription
- **Hinglish** — Hindi + English mix (Sarvam AI)

### 3. Click "Analyse"
Watch real-time pipeline:
- 🔊 **Audio Processing** — Extract audio from video, split into chunks
- 📝 **Transcription** — Convert speech to text (Whisper)
- 🏷️ **Title Generation** — AI-generated meeting title (Mistral)
- 📋 **Summarisation** — Concise 3-5 line summary
- 🔍 **Extraction** — Action items, key decisions, open questions
- 🧠 **RAG Engine** — Build vector store for Q&A

### 4. View Results & Chat
- **Summary** — AI-generated executive summary
- **Transcript** — Full meeting transcript (expandable)
- **Action Items** — Extracted tasks
- **Key Decisions** — Important decisions made
- **Open Questions** — Unanswered questions
- **💬 Chat** — Ask questions about the meeting

**Example questions:**
- "What were the main decisions made?"
- "List all action items"
- "Who needs to do what by when?"

---

## 📁 Project Structure

```
AI-Video-Assistant-App/
├── core/
│   ├── __init__.py
│   ├── audio_processor.py        # Audio extraction utilities
│   ├── transcriber.py             # Whisper integration
│   ├── summarizer.py              # Title & summary generation (Mistral)
│   ├── extractor.py               # Action items, decisions, questions
│   ├── rag_engine.py              # RAG chain with ChromaDB
│   └── vector_store.py            # Vector store management
│
├── utils/
│   ├── __init__.py
│   └── audio_processor.py          # Audio chunk processing (entry point)
│
├── app.py                          # Streamlit UI
├── main.py                         # CLI interface (optional)
├── test.py                         # Pipeline tests
├── requirements.txt                # Dependencies
├── runtime.txt                     # Python version (for deployment)
├── .env                            # API keys (not in git)
├── .gitignore                      # Git ignore rules
├── .streamlit/
│   └── config.toml                # Streamlit configuration
└── README.md                       # This file
```

---

## 🔄 Pipeline Flow

```
User Input (Video/Audio)
          ↓
  Audio Extraction (FFmpeg)
          ↓
    Audio Chunking (16 chunks)
          ↓
Transcription (Whisper Model)
          ↓
    Transcript Post-processing
          ↓
     Title Generation (Mistral)
          ↓
 Summarization (Mistral)
          ↓
Extraction (Action Items, Decisions, Q&A)
          ↓
 RAG Engine Setup (ChromaDB)
          ↓
User Can Now Chat with Transcript
```

---

## 📊 Performance

| Task | Time | Model |
|------|------|-------|
| Audio Extraction | 2-5s | FFmpeg |
| Transcription (10 min video) | 30-60s | Whisper small |
| Summarization | 5-10s | Mistral 7B |
| RAG Setup | 3-5s | ChromaDB |
| **Total (10 min)** | **~1-2 min** | All combined |

*Timings vary based on file size, internet speed, GPU availability*

---

## ⚙️ Configuration

### Streamlit Settings (`.streamlit/config.toml`)
```toml
[client]
maxUploadSize = 500  # 500 MB file upload limit

[server]
maxUploadSize = 500
```

### Whisper Model Options
```
- small    (77M params)    — Fastest, good accuracy
- medium   (387M params)   — Slower, better accuracy
- large    (1.5B params)   — Slowest, best accuracy
```

Change in `.env`: `WHISPER_MODEL=medium`

---

## 🐛 Troubleshooting

### "ModuleNotFoundError: No module named 'ffmpeg'"
```bash
# Install FFmpeg
# macOS
brew install ffmpeg

# Ubuntu/Debian
sudo apt-get install ffmpeg

# Windows
# Download from ffmpeg.org or use: choco install ffmpeg
```

### "CUDA out of memory" (if using GPU)
- Reduce `WHISPER_MODEL` to `small`
- Or force CPU: `CUDA_VISIBLE_DEVICES="" streamlit run app.py`

### "API key invalid"
- Verify `.env` file exists and keys are correct
- Check API key has proper permissions
- Regenerate key if expired

### "Connection timeout on YouTube"
- **Local only** — YouTube blocks cloud servers (industry-wide)
- Use file upload instead for cloud deployment
- Local YouTube works fine with `yt-dlp`

---

## 📝 Deployment Notes

### ✅ Local (Recommended for Portfolio)
```bash
streamlit run app.py
```
- ✅ All features work (YouTube, large files)
- ✅ 500 MB file support
- ✅ Instant processing

### ☁️ Cloud Deployment (Limitations)

**Render Free Tier** (current):
- ❌ YouTube blocked (Render IP blacklisted)
- ⚠️ Large files (>50 MB) may timeout due to 512 MB memory limit
- Use for demos only

**Paid Tiers** ($7+/month):
- ✅ More memory (1 GB+)
- ✅ Reliable file uploads
- ❌ YouTube still blocked (global policy)

**To upgrade:** Render Dashboard → Instance Type → Standard ($7/month)

---

## 🎯 Features Explained

### RAG (Retrieval-Augmented Generation)
```
User Question: "What were action items?"
              ↓
        Vector Search (ChromaDB)
              ↓
    Retrieve relevant transcript chunks
              ↓
   Pass to Mistral with context
              ↓
  Get intelligent answer based on transcript
```

### Hinglish Support
- **English mode:** Standard English transcription
- **Hinglish mode:** Recognizes Hindi words mixed with English
- Uses Sarvam AI's `saaras:v2.5` model

### Real-time Status
Pipeline shows live progress:
- 🟡 Active (pulsing) — Currently processing
- 🟢 Done (solid) — Completed
- ⚪ Pending — Waiting to start

---

## 🚀 Future Improvements

- [ ] YouTube integration (with cookies for auth)
- [ ] Support for meetings/conference recordings
- [ ] Export to PDF/Word
- [ ] Team collaboration features
- [ ] Custom prompt templates
- [ ] Batch processing
- [ ] API endpoint
- [ ] Mobile app

---

## 📚 Learning Resources

This project demonstrates:
- **LLM Integration:** Mistral API, OpenAI Whisper
- **RAG Systems:** ChromaDB + LangChain LCEL
- **Streamlit UI:** Custom CSS, real-time updates
- **Audio Processing:** FFmpeg, moviepy, pydub
- **Python Best Practices:** Modular code, error handling
- **Deployment Considerations:** API keys, memory limits, cloud constraints

---

## 🤝 Contributing

Improvements welcome! Areas:
- Better error handling
- Additional language support
- UI/UX enhancements
- Performance optimization
- Tests (unit, integration)

Fork → Create feature branch → Submit PR

---

## 📄 License

MIT License — See LICENSE file for details

---

## 👤 Author

**Faraz** — [GitHub](https://github.com/mhd-faraz) | [Portfolio](https://faraz-portfolio-ivvo-xi.vercel.app)

---

## 🙏 Acknowledgments

- OpenAI (Whisper)
- Mistral AI
- Langchain Community
- Streamlit
- ChromaDB
- Sarvam AI

---

## 📞 Support

Issues or questions? Open a GitHub issue or reach out.

**Status:** ✅ Fully functional locally | Portfolio-ready

---

**Last Updated:** June 2026 | Python 3.12 | Streamlit 1.38+