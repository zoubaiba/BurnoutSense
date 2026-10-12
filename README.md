# BurnoutSense: PsychoBot Core Engine

Standalone **AI engine** of *Zenwork*, a well-being app built as a 4-month full-stack university group project (React, Django, Supabase).
I was responsible for the AI layer: LLM integration, prompt engineering and burnout risk scoring. This repo extracts that work so it can run **without the database or the frontend**.

> ⚠️ Technical demo only. Scores are LLM estimates, not clinically validated. Not a medical tool.

## What it does

| Function | Role |
|---|---|
| `compute_burnout_score` | Normalizes questionnaire answers (emojis, 1–10 scales, yes/no, free text) and asks the LLM for a **strict JSON** result: risk score (0–100) + short reasons |
| `chat_with_gemini` | *(name kept for compatibility with the original Django views, runs on Groq)* **PsychoBot**, an empathetic French-speaking assistant: short replies, one question at a time, conversation memory through the message history and a hidden context built from the questionnaire (`build_context_prefix`) |
| `update_score_from_chat` | Re-reads the whole conversation and **refines the score**; reasons are requested vague and anonymized |
| `detect_besoins_ressources` | Detects the need (stress, sleep, mental health, sport, meditation), an **urgency flag** and a redirection message |
| `generate_commentaire_rh` | One-sentence anonymized summary of the employee's state |
| `determine_level` | Maps a score to a level: none (<20), low (20–44), medium (45–69), high (≥70) |

Prompts and conversations are in **French**, as Zenwork targets French-speaking employees.

## Design notes

- **Reliable output:** `response_format={"type": "json_object"}` + parsing and safe fallbacks
- **Stateless service:** the caller passes the history; nothing is stored by this engine
- **Privacy:** anonymization is enforced through prompt instructions (no guarantee), and data is sent to the Groq API, so a production version would need a GDPR review

## Limitations

- On an API error, `compute_burnout_score` returns a neutral fallback score (50) with an error message
- Severe-distress handling relies on the system prompt and an LLM urgency flag, with no rule-based safeguard
- Scoring calls use the default temperature, so results can vary slightly between runs
- Not production-ready: a real deployment would need clinical validation and a GDPR review

## Tech stack

Python 3 · Groq API · LLaMA 3.3 (70B Versatile) · Flask

## Quick start

```bash
git clone https://github.com/zoubaiba/BurnoutSense.git
cd BurnoutSense
pip install -r requirements.txt
echo "GROQ_API_KEY=your_key" > .env
python demo.py
```

The demo runs 3 steps: questionnaire scoring → 3-message chat → score refinement.

## Team

Group project at Aix-Marseille Université. 
My part: AI engine, prompt engineering, risk scoring.

## Contact

Lylia Ait Mokhtar · [GitHub](https://github.com/zoubaiba) · [LinkedIn](https://www.linkedin.com/in/lylia-ait-mokhtar-502609424) · lyliaaitm@gmail.com
