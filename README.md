# Pairwise — AI-Powered Resume Matching Engine

Semantic resume-to-job matching using vector embeddings and LLM-generated fit evidence.
Built to replace slow keyword-based ATS filtering with real-time AI ranking for technical roles.

**Arizona State University — CIS 568: AI Systems | Group Project, Spring 2026**

***

## The Problem

Traditional ATS systems filter candidates by keyword overlap — a resume missing the exact phrase "distributed systems" gets rejected even if the candidate built one. Pairwise uses semantic vector similarity to match meaning, not keywords. The result is a ranking system that surfaces the right people instead of the right keywords.

***

## What I Built

A full-stack AI matching application with a FastAPI backend and React frontend:

- **Semantic matching engine** — sentence-transformers (all-MiniLM-L6-v2) encodes resumes and job descriptions into the same vector space, then cosine similarity ranks candidates by true meaning rather than keyword overlap
- **LLM-generated fit evidence** — Groq (llama-3.3-70b) explains exactly why each candidate scored the way it did, giving recruiters 3 specific evidence bullets per match
- **Bias mitigation layer** — demographic signals are stripped from the vector matching step before any score is computed; low-confidence matches are flagged for human review instead of auto-rejected
- **ATS-ready JSON output** — match scores and fit evidence are returned as a typed Pydantic payload that plugs into Greenhouse, Lever, or Ashby without touching vendor code
- **4-screen React UI** — Landing → Resume Upload → Job Requirements → Ranked Results dashboard

***

## Demo



***

## Architecture

| Folder / File | Purpose |
|---|---|
| backend/ | Python 3.11 + FastAPI application root |
| backend/app/api/routes.py | POST /api/v1/match endpoint |
| backend/app/models/schemas.py | Pydantic models: MatchPayload, FitEvidence, ATSTag |
| backend/app/services/embeddings.py | sentence-transformers vector encoding |
| backend/app/services/groq_client.py | Groq LLM integration for fit evidence generation |
| backend/app/services/matcher.py | Core logic: embed, cosine rank, LLM explanation |
| frontend/ | React 18 + Vite + Tailwind CSS |
| frontend/src/pages/ | Landing, Upload, Requirements, Results screens |

***

## How to Run

**Backend**

    cd backend
    pip install -r requirements.txt
    uvicorn app.main:app --reload

**Frontend**

    cd frontend
    npm install
    npm run dev

Open http://localhost:5173 in your browser. The API runs on http://localhost:8000.

***

## Tech Stack

- Python 3.11, FastAPI, Pydantic v2
- sentence-transformers — all-MiniLM-L6-v2
- Groq API — llama-3.3-70b-versatile
- React 18, Vite, Tailwind CSS
- Cosine similarity for semantic ranking

***

## Key Design Decisions

**Why sentence-transformers over OpenAI embeddings?** Local inference means zero API cost during development and no data leaving the environment before scoring.

**Why Groq for evidence generation?** Groq's inference speed (llama-3.3-70b at ~800 tokens/sec) keeps the full matching pipeline under 2 seconds per request, which matters for real-time recruiter workflows.

**Why flag low-confidence matches instead of filtering them?** Hard cutoffs hide edge cases. Flagging keeps the human in the loop for borderline candidates rather than silently auto-rejecting them.

***

## Status

Active development — Spring 2026. Core matching engine and bias mitigation layer complete. ATS JSON integration in testing.
