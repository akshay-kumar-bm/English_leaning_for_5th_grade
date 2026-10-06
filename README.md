# English Reading Adventure

**A Streamlit app that uses GPT-4o-mini to generate graded reading passages, vocabulary, comprehension questions and quizzes, with slow text-to-speech and a points system.**

## What it does

Young learners get a short passage with new words, can listen to it sentence by sentence (gTTS audio with adjustable speed), and earn points for reading. Content is generated with LangChain structured output and cached in a rotating pool of passages.

## Features (from `main.py`)

- Structured LLM output via Pydantic models: passage plus vocabulary, 3 comprehension questions, 3 multiple-choice vocabulary quiz questions
- Pool of up to 5 passages stored in `data/passages.json`, served in rotation; a new one is generated in a background thread after a passage is finished
- Per-sentence and full-passage text-to-speech (gTTS) with a speech-speed slider
- Reading timer: 2 points only if the learner stays at least 60 seconds
- Progress saved to `data/user_progress.json`; badge/"Master Reader" messages at 50/100 points
- `data/topic.json` holds a list of child-friendly topics

## Tech stack

Python, Streamlit, LangChain (`langchain`, `langchain_openai`), OpenAI `gpt-4o-mini`, Pydantic, gTTS, python-dotenv.

## Structure

```
main.py              # current app (v2: JSON pool, background generation, TTS controls)
app.py               # earlier prototype (gpt-3.5-turbo, session-state only)
data/                # topic.json, passages.json, user_progress.json
requirements.txt
```

## Run

```bash
pip install -r requirements.txt
# .env (git-ignored): OPENAI_API_KEY=...   (or st.secrets["openai_api_key"])
streamlit run main.py
```

## Limitations

- Titles and prompts are inconsistent (repo name says 5th grade, UI title says 8th graders, prompts say "2th" grade).
- Comprehension questions and quiz are generated and stored but not yet rendered in `main.py`.
- The passage pool must be seeded (initializer is commented out); no tests; progress is a single local JSON, so no multi-user support.
