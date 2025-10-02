# Recruiter Screening Notes — LangChain + LangGraph + Gradio

A lightweight workflow that turns messy recruiter screening-call notes into **polished Screening Notes** and **5 tailored job-title suggestions**—in minutes, not hours. Built with LangChain for LLM tooling and LangGraph for a deterministic, debuggable graph.

## Why
Recruiters spend time wrangling unstructured notes into structured fields and summaries. This project automates the tedious parts while keeping you in control and easy to customize for your roles, style guide, and ATS.

## What it does
- **Structured extraction** from raw notes into a Pydantic model (`ScreeningExtraction`)—no invented fields.
- **Skill normalization** via alias/fuzzy matching (RapidFuzz).
- **Top-5 title recommendations** with brief rationales and calibrated confidence.
  - Includes **AI Engineer** and **Prompt Engineer** out of the box.
- **Outputs**
  - **Markdown**: formatted “Screening Notes” for ATS/CRM.
  - **JSON**: structured data for automations/analytics.
- **Gradio UI**
  - View-only mode (no files saved).
  - Optional downloads toggle (create temp files only when enabled).

## How it works (LangGraph)
```
Raw Notes
   ↓
Preprocess (clean text)
   ↓
Extract (LLM → ScreeningExtraction schema)
   ↓
Normalize Skills (aliases + fuzz)
   ↓
Suggest Titles (keyword score + LLM rationale)
   ↓
Format Markdown + JSON
```

## Quick start
```bash
pip install "langchain>=0.2.10" "langgraph>=0.2.20" "langchain-openai>=0.1.20" \
            "pydantic>=2.7.0" "rapidfuzz>=3.7.0" "python-dotenv>=1.0.1" gradio>=4.44.0
```

Set your model key (OpenAI shown; swap providers as you wish)

### Run Notebook
1) Open the notebook (core or Gradio version).  
2) Paste your screening notes into `RAW_NOTES` **or** use the Gradio UI.  
3) Execute the “Run graph” / “Launch Gradio app” cell.

### Run (Gradio UI only, view-only)

## Configuration
- **Model/provider:** edit `make_llm()` (model name, temperature, provider).
- **Title ontology:** update `TITLE_ONTOLOGY` / `TITLE_KEYWORDS` (includes AI Engineer & Prompt Engineer).
- **Skills:** extend `CANONICAL_SKILLS` for better normalization (aliases/fuzzy).
- **Output:** tweak Markdown assembly in `format_markdown_node`.

## Design choices
- **Deterministic stages:** LangGraph separates extraction, normalization, scoring, and formatting for easy debugging.
- **Structured output:** `with_structured_output(ScreeningExtraction)` enforces schema and reduces drift.
- **Prompt safety:** JSON schemas are passed as template **variables** to avoid brace/templating errors.

## Roadmap
- Audio → text (Whisper) for auto-transcription  
- Lever/Greenhouse integration to push JSON  
- Bias/PII redaction pass before formatting  
- Learning loop: use hiring outcomes to refine title suggestions

## Stack
LangChain • LangGraph • Pydantic • RapidFuzz • Gradio • OpenAI (Model of your choice)

---

**TL;DR:** Paste screening notes, get clean, consistent summaries and smart job-title suggestions—fast, auditable, and easy to tailor to your workflow.
