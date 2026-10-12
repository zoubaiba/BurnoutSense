# BurnoutSense: PsychoBot Core Engine

Standalone **AI engine** of *Zenwork*, a well-being app built as a 4-month full-stack university group project (React, Django, Supabase).
I was responsible for the AI layer: LLM integration, prompt engineering and burnout risk scoring. This repo isolates that work as a lightweight **Flask API** with no database or frontend dependencies.

> ⚠️ Technical demo only. Not a medical tool and not a substitute for professional support.

## Features

- **`/analyze`**: turns questionnaire data (emojis and free text included) into a burnout risk score (0–100) and anonymized signals
- **`/chat`**: empathetic conversational agent ("PsychoBot") with memory of the user's emotional state
- **`/session/close`**: refines the initial score from the full conversation (hidden distress, reassuring cues)
- **Resource triggering**: detects urgent needs (sleep, stress…) and suggests resources or emergency redirects

Key techniques: LLM orchestration (context, system prompts, memory), strict **JSON-structured outputs** for reliable frontend parsing, no identifiable personal data stored.

## Tech stack

Python 3 · Flask · Groq API · LLaMA 3.3 (70B Versatile)

## Getting started

```bash
git clone https://github.com/zoubaiba/BurnoutSense.git
cd BurnoutSense
pip install -r requirements.txt
echo "GROQ_API_KEY=your_key" > .env
python [fichier_principal.py]
```

Example:

```bash
curl -X POST http://localhost:5000/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "I feel overwhelmed at work lately."}'
```

## Team

Group project at Aix-Marseille Université
My part: AI engine, prompt engineering, risk scoring.

## Contact

Lylia Ait Mokhtar · [GitHub](https://github.com/zoubaiba) · [LinkedIn](https://www.linkedin.com/in/lylia-ait-mokhtar-502609424) · lyliaaitm@gmail.com
