
# TubeMate AI 


This is an AI project built with Python that provides an interactive interface using Gradio to analyze YouTube videos. It converts video, summarizes the content, translates, and allows users to ask questions about the video through an intelligent chatbot.

---

## Features

- **Context-Aware QA** using **Chroma Vector Store** (via LangChain)
- **Audio Transcription** via **OpenAI Whisper API**
- **Smart Summarization** using **GPT-3.5 / GPT-4**
- **Multilingual Translation** via `translate_text()`
- **Interactive Chatbot** powered by **LangChain Agent**
- **Real-time Vector Embedding** using **OpenAIEmbeddings**
- **User Interface** with **Gradio Blocks + Tabs**
- **Developer Tracing & Evaluation** via **LangSmith** (optional)


---

## Vector Search & QA Pipeline

This app uses **Chroma** (via LangChain) as a vector database to store transcript chunks of the video. When a user asks a question:

1. The transcript is split into chunks using `TokenTextSplitter`.
2. Each chunk is embedded using `OpenAIEmbeddings`.
3. Chunks are stored in a **Chroma Vector Store**.
4. On question input:
   - A **RetrievalQA** chain fetches relevant chunks.
   - GPT (with `stuff` or `map_reduce`) generates the response.
   - Timestamp questions are handled manually via segment metadata.
5. Timestamp-related questions are handled manually using segment metadata.

---

##  Project Structure & Code Overview

```
youtube_project/
│
├── main.py
│   - The app’s main entry point.
│   - Runs Gradio UI via `build_interface()`from `gradio_ui.py`.
│
├── gradio_ui.py
│   - Builds the Gradio interface using `gr.Blocks`, tabs, buttons, and toggles.
│   - Connects UI actions (e.g., “Submit”, “Translate”) to backend functions.
│
├── bot.py
│   - Core orchestrator file.
│   - Uses:
│     - `load_video(url)` → transcribes audio and summarizes using GPT.
│     - `run_agent_tool(command)` → answers user questions using LangChain.
│   - Stores global `video_state` for QA and timestamps.
│
├── audio_utils.py
│   - Handles audio post-processing, e.g., converting audio formats.
│   - Used when downloading/processing YouTube audio.
│
├── translate.py
│   - Contains `translate_text(text, target_lang)` to translate summaries.
│   - Uses Google Translate API or custom translation logic.
│
│
├── utils/
│   ├── transcriber.py
│   │   - Uses OpenAI Whisper API for transcribing audio.
│   │   - `transcribe_audio(audio_path)` sends audio to Whisper.
│   │
│   └── summarizer.py
│       - Uses OpenAI GPT (e.g., GPT-4) to summarize transcripts.
│       - `summarize_transcript(text, duration)` gives a structured summary.
```

---
## LangSmith
- **LangSmith** (optional) can be used for:
  - Tracing and debugging agent behavior
  - Evaluating responses and prompt chains

##  Requirements

Install dependencies:

```bash
pip install -r requirements.txt
```

Or manually install:

```bash
pip install gradio openai gtts python-dotenv yt_dlp
```

---

## Setup

1. **Clone the project** or extract it.
2. **Set up your `.env` file**:
```
LANGCHAIN_API_KEY=your_langsmith_api_key_here
OPENAI_API_KEY=your_openai_api_key_here
```
3. **Run the app**:
```bash
python main.py
```

---

## How to Use

1. Paste a YouTube URL.
2. Click **Submit** to transcribe and summarize.
3. View the summary and transcript.
4. Translate the summary if needed.
5. Ask questions about the video in the chatbot.


The Demo:
https://drive.google.com/file/d/1UaBI_jboZhSTTCf3NnThqtBiskTo5L14/view?usp=sharing

---

##  Notes

- Make sure your OpenAI API key is valid.
- This app uses **Whisper via OpenAI API** (not local models).
- GPT-4 is used for intelligent summarization and chatbot responses.
- Translations are handled separately with `translate_text`.


