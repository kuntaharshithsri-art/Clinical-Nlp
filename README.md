---
title: Clinical NLP Summarizer
emoji: 🏥
colorFrom: blue
colorTo: indigo
sdk: streamlit
sdk_version: "1.32.0"
python_version: "3.10"
app_file: app.py
pinned: false
---

<div align="center">

# 🏥 Clinical NLP Summarizer

### AI-Powered Clinical Intelligence System

[![Live Demo](https://img.shields.io/badge/🚀%20Live%20Demo-HuggingFace%20Spaces-blue?style=for-the-badge)]
 [https://huggingface.co/spaces/kuntaharshithsri-art/clinical-nlp-summarizer](https://huggingface.co/spaces/khemant11/clinical-nlp-summarizer)

[![Python](https://img.shields.io/badge/Python-3.10-green?style=for-the-badge&logo=python)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.32-red?style=for-the-badge&logo=streamlit)](https://streamlit.io)
[![HuggingFace](https://img.shields.io/badge/🤗%20HuggingFace-Transformers-yellow?style=for-the-badge)](https://huggingface.co)

**🌐 Live App: [https://huggingface.co/spaces/khemant11/clinical-nlp-summarizer](https://huggingface.co/spaces/khemant11/clinical-nlp-summarizer)**

</div>

---

## 📌 Overview

The **Clinical NLP Summarizer** is a full-stack AI-powered web application that helps clinicians and medical staff quickly analyze and summarize patient medical records. It uses state-of-the-art NLP models from HuggingFace to extract insights, detect medical entities, assess risk, and answer clinical questions — all from raw text or uploaded documents.

---

## ✨ Features

| Feature | Description |
|---|---|
| 📄 **OCR Extraction** | Upload patient images (PNG/JPG) or scanned PDFs and extract text using Tesseract OCR |
| 🧠 **AI Summarization** | Section-aware, structured clinical narrative using rule-based NLP + HPI parsing |
| 🔬 **Biomedical NER** | Detects Diseases, Medications, Procedures, and Symptoms using `d4data/biomedical-ner-all` |
| ⚠️ **Risk Triage** | Automated 3-level triage: 🔴 Critical / 🟠 Urgent / 🟢 Routine with trigger detection |
| 🤖 **Clinical Q&A** | Ask any question about the patient record powered by `deepset/roberta-base-squad2` |
| 📕 **PDF Report** | Download an official formatted PDF report with risk, summary and all entities |
| 📊 **Patient Dashboard** | Sidebar stats showing patient traffic history and recent records |

---

## 🏗️ Project Architecture

```
Clinical_NLP_Summarizer/
│
├── app.py              # Main Streamlit UI — all pages, layout, CSS, session state
├── ai_engine.py        # Core AI logic — summarization, NER, risk scoring, Q&A, OCR
├── db_manager.py       # SQLite database — save/retrieve summaries and patient stats
├── report_gen.py       # PDF report generator using fpdf
│
├── requirements.txt    # Python dependencies
├── packages.txt        # System packages (tesseract-ocr for Linux/HF Spaces)
└── README.md           # This file
```

---

## 🤖 AI Models Used

| Task | Model | Source |
|---|---|---|
| **Text Summarization** | `facebook/bart-large-cnn` | HuggingFace |
| **Biomedical NER** | `d4data/biomedical-ner-all` | HuggingFace |
| **Clinical Q&A** | `deepset/roberta-base-squad2` | HuggingFace |
| **NLP Pipeline** | `en_core_web_sm` | spaCy |
| **OCR** | Tesseract OCR | Open Source |

---

## 🧠 How the AI Works

### 1. 📥 Input

The user can either:
- **Paste raw text** — patient notes, discharge summaries, clinical records
- **Upload a file** — image (PNG/JPG) or PDF → text extracted via **Tesseract OCR**

### 2. 🧬 Summarization (`ai_engine.py → summarize_medical_text`)

A 10-step rule-based clinical summarizer that:
- **Segments** the document into clinical sections (HPI, PMH, Vitals, Exam, Assessment, Plan)
- **Extracts demographics** (name, age, gender) from free text
- **Parses HPI** (duration, chief complaint, character, radiation, aggravating/relieving factors)
- **Identifies past medical/surgical history** with year timestamps (26 condition patterns)
- **Detects family history** and **social history** (smoking, alcohol, HRT)
- **Reads vitals** (BP, HR, RR, Temp, SpO2)
- **Finds physical exam findings** (murmurs, crackles, edema, JVP, etc.)
- **Extracts assessment items** from numbered lists
- Assembles everything into **smooth clinical paragraph narrative**

### 3. 🔍 Named Entity Recognition (`ai_engine.py → get_entities`)

- Primary: `d4data/biomedical-ner-all` transformer model
- Fallback: Comprehensive rule-based lexicon (30+ diseases, 20+ medications, 20+ procedures)
- Entities categorized as: **Disease/Disorder**, **Medication**, **Diagnostic Procedure**, **Sign/Symptom**

### 4. ⚠️ Risk Triage (`ai_engine.py → calculate_risk_score`)

Keyword-based scoring engine:
- **Critical (+3 each)**: cardiac arrest, stroke, severe chest pain, seizure, high fever (103°F+)
- **Urgent (+2 each)**: fracture, bleeding, fever, shortness of breath, severe pain
- **Routine (+1 each)**: nausea, dizziness, cough, headache, mild pain
- Score ≥ 5 → 🔴 **CRITICAL** | Score ≥ 2 → 🟠 **URGENT** | Score < 2 → 🟢 **ROUTINE**

### 5. 🤖 Q&A (`ai_engine.py → answer_question`)

Extractive question answering using `deepset/roberta-base-squad2`:
- Takes the patient record as context (up to 4000 characters)
- Answers any clinical question in natural language
- Returns confidence score and low-confidence warnings

### 6. 📕 PDF Report (`report_gen.py → create_pdf`)

Generates a formatted PDF with:
- Risk Assessment section
- Clinical Summary section
- Key Medical Entities section

### 7. 💾 Database (`db_manager.py`)

SQLite3 database with:
- `summaries` table storing: original text, generated summary, timestamp
- Patient traffic statistics for the dashboard chart
- Session history displayed in the sidebar (last 5 records)

---

## 🖥️ UI Design

Built with **Streamlit** and custom CSS:
- **Dark navy theme** with glassmorphism cards
- **Animated components** (fadeSlideDown, fadeSlideUp, pulseIn)
- **Color-coded risk cards**: Red (Critical), Orange (Urgent), Green (Routine)
- **Entity badges**: Color-coded by type
- **3-tab results panel**: Summary | NER | Q&A
- Fully **responsive layout** with sidebar dashboard

---

## 🚀 Run Locally

### Prerequisites
- Python 3.10+
- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) installed on your system

### Setup

```bash
# 1. Clone the repository
git clone https://huggingface.co/spaces/khemant11/clinical-nlp-summarizer
cd clinical-nlp-summarizer

# 2. Create virtual environment
python -m venv venv
venv\Scripts\activate     # Windows
# source venv/bin/activate  # Mac/Linux

# 3. Install dependencies
pip install -r requirements.txt

# 4. Set Tesseract path (Windows only) — edit ai_engine.py line 14
# pytesseract.pytesseract.tesseract_cmd = r'C:\Path\To\tesseract.exe'

# 5. Run the app
streamlit run app.py
```

App will open at: `http://localhost:8501`

---

## 📦 Dependencies

```
streamlit>=1.32.0        # Web UI framework
transformers>=4.38.0     # HuggingFace NLP models
torch>=2.2.0             # Deep learning backend
spacy>=3.8.0             # NLP pipeline (en_core_web_sm)
pytesseract>=0.3.10      # OCR engine wrapper
Pillow>=10.2.0           # Image processing
pdfplumber>=0.10.3       # PDF text extraction
fpdf>=1.7.2              # PDF report generation
pandas>=2.2.0            # Data handling for dashboard
```

---

## � Example Use Cases

- **Emergency Room Triage** — paste patient notes to instantly get risk level
- **Clinical Documentation** — upload scanned records and get structured summaries
- **Medical Education** — ask questions about complex patient cases
- **Report Generation** — generate official PDF reports from AI analysis

---

## ⚠️ Disclaimer

> This tool is intended for **educational and demonstration purposes only**. It is not a substitute for professional medical diagnosis or clinical judgment. Always consult a qualified healthcare professional for medical decisions.

---

## 👨‍💻 Author

**K.Harshith Sri** · [@kuntaharshithsri-art](https://huggingface.co/khemant11)

🌐 **Live Demo**: [https://huggingface.co/spaces/kuntaharshithsri-art/clinical-nlp-summarizer](https://huggingface.co/spaces/khemant11/clinical-nlp-summarizer)

---

<div align="center">
Built with ❤️ using Streamlit · HuggingFace Transformers · spaCy · Tesseract OCR
</div>
