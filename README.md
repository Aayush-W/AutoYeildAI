# AutoYield-AI

**AutoYield AI** is an autonomous wafer quality pipeline that combines computer vision inference, model explainability, drift monitoring, and GenAI-assisted root-cause analysis. It is built as a full-stack monorepo with a FastAPI backend, a React/Vite dashboard, and a Streamlit demo UI.

---

## Overview

### AutoYield AI: Intelligent Yield Optimization for Semiconductor Manufacturing

The more we studied semiconductor fabrication, the more one fact stood out:

> By the time a defect is discovered, the damage is already done.

A wafer has already passed through dozens of processing stages. It has consumed significant energy, expensive chemicals, and thousands of liters of ultra-pure water. Engineers must pause production, investigate logs, analyze inspection images, and trace the root cause across a highly complex manufacturing process.

We kept asking ourselves:

- What if this investigation could begin before the wafer fails?
- What if inspection data could actively help engineers prevent waste instead of simply explaining it afterward?
- What if engineers could interact with an intelligent assistant that understands manufacturing history and process knowledge?
- What if they could test corrective actions in a virtual environment before applying them on the production floor?

**AutoYield AI** was born from the desire to reduce engineering effort, financial losses, and environmental waste simultaneously.

---

## What AutoYield AI Does

AutoYield AI analyzes wafer inspection images to detect defect patterns early and assist engineers in understanding what might go wrong next.

The platform combines:

- **Computer vision** for defect detection
- **Continual learning** for adaptive model improvement
- **Digital Twin simulation** for virtual process experimentation
- **Retrieval-Augmented Generation (RAG)** for grounded engineering intelligence

The system is designed to evolve after deployment. When AutoYield encounters a defect it is uncertain about, it learns from that experience. Over time, the platform becomes more capable of recognizing rare, evolving, and previously unseen defect patterns.

Rather than treating inspection data as historical information, AutoYield transforms it into a continuously improving decision-support system for yield engineers.

---

## How It Works

### 1. Defect Detection Engine

AutoYield uses a two-model architecture:

- **ConvNeXt-Small (Base Model)**
  - High-accuracy foundation model
  - Trained on curated wafer defect datasets
  - Achieves approximately **92% classification accuracy**
  - Provides robust baseline defect detection

- **EfficientNet (Adaptive Model)**
  - Lightweight and computationally efficient
  - Retrains quickly on newly discovered defects
  - Continuously adapts to changing manufacturing conditions

### 2. GAN-Based Data Augmentation

Rare defects are difficult to learn due to limited training samples. AutoYield uses a **Generative Adversarial Network (GAN)** to generate realistic synthetic defect variations.

These synthetic samples help the system:

- Improve generalization
- Handle class imbalance
- Learn emerging defect patterns faster
- Increase detection robustness

### 3. Continual Learning Loop

Whenever the system detects a low-confidence defect:

1. The image is reviewed and correctly labeled.
2. GAN-generated defect variations are created.
3. The EfficientNet model is retrained using the expanded dataset.
4. Future predictions improve automatically.

This creates a self-learning feedback loop without retraining the full base model, significantly reducing computational cost and deployment complexity.

### 4. Digital Twin for Process Simulation

AutoYield incorporates a **Digital Twin** of the semiconductor manufacturing process.

The Digital Twin is a virtual replica of the fabrication environment, continuously updated using real inspection data, process parameters, and defect observations.

Engineers can use the Digital Twin to:

- Simulate process parameter changes
- Predict potential yield impacts
- Evaluate defect propagation risks
- Compare optimization strategies
- Reduce costly trial-and-error experiments

This enables informed decisions before affecting physical wafers, reducing waste and improving production efficiency.

### 5. RAG-Based AI Engineering Assistant

AutoYield includes a **Retrieval-Augmented Generation (RAG)** assistant tailored for semiconductor manufacturing.

The assistant retrieves information from:

- Historical defect cases
- Process documentation
- Engineering reports
- Equipment logs
- Standard operating procedures
- Internal knowledge bases

Engineers can ask questions like:

- “Why are scratch defects increasing in Lot A?”
- “What process steps are commonly associated with ring defects?”
- “Have we observed similar failures before?”
- “Which corrective action produced the best results historically?”

Instead of producing generic answers, the RAG system grounds responses in actual factory knowledge and historical evidence, delivering explainable and actionable recommendations.

---

## Sustainability Impact

AutoYield AI is designed with sustainable manufacturing in mind.

By detecting defects earlier, predicting failures, and enabling virtual experimentation through the Digital Twin, manufacturers can avoid unnecessary processing and rework.

The platform helps reduce:

- **Material waste**
- **Energy consumption**
- **Water usage**
- **Chemical usage**
- **Carbon emissions**

It also supports tracking of:

- Energy saved
- Water conserved
- Material waste prevented
- Carbon emissions avoided

---

## Vision

Our vision is to transform wafer inspection from a passive quality-control step into an intelligent, self-improving decision system.

By combining advanced computer vision, continual learning, Digital Twin simulation, and RAG-powered engineering intelligence, AutoYield AI helps semiconductor manufacturers:

- Improve yield
- Reduce costs
- Accelerate root-cause analysis
- Move toward a more sustainable future

---

## Tech Stack

- **Backend:** FastAPI, PyTorch, torchvision, OpenCV, NumPy, scikit-learn
- **Frontend:** React, Vite, React Router
- **Optional GenAI:** Google Gemini via `google-generativeai`

---

## Workspace Structure

```text
AutoYeildAI/                    (Root Workspace)
├── client/                      (Frontend Application - React + Vite)
├── server/                      (Backend Application - FastAPI + ML Pipelines)
└── docs/                        (Overall system architecture and documentation)
```

---

## Quick Start

### 1) Backend API (FastAPI)

```bash
cd server
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
pip install torch torchvision opencv-python pillow
```

Create `server/.env` from `server/.env.example`, then start the API:

```bash
uvicorn api.app:app --reload --port 8000
```

Recommended endpoints:

- `POST /api/analyze` — analyze wafer images
- `GET /api/history` — fetch recent inspections
- `GET /api/metrics` — retrieve dashboard metrics

### 2) Web Dashboard (React + Vite)

```bash
cd client
npm install
npm run dev
```

The dashboard expects the backend API at `http://localhost:8000`.

### 3) Streamlit Demo

```bash
cd server
streamlit run ui/dashboard.py
```

---

## Environment Variables

### Backend (`server/.env`)

```env
MONGO_URI=mongodb+srv://YOUR_DB_USER:YOUR_DB_PASSWORD@cluster0.0dwbwfe.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0
MONGO_DB_NAME=autoyield
MONGO_SERVER_SELECTION_TIMEOUT_MS=5000
CORS_ORIGINS=http://localhost:5173,http://127.0.0.1:5173
```

> If your MongoDB password contains special characters such as `@`, `:`, `/`, `?`, `#`, or `%`, URL-encode it.

### Frontend (`client/.env`)

```env
VITE_API_BASE_URL=http://localhost:8000
```

### Optional Gemini Root-Cause Analysis

```env
GEMINI_API_KEY=your_key_here
GEMINI_MODEL=gemini-1.5-flash
```

If Gemini is not configured, the system falls back to deterministic root-cause analysis rules.

---

## Project Structure Detail

- `client/` — React/Vite frontend dashboard
- `server/` — Backend workspace
  - `api/` — FastAPI service
  - `src/` — Core ML pipeline (inference, explainability, drift, reasoning)
  - `scripts/` — RAG ingestion and indexing utilities
  - `config/` — YAML configs and prompt templates
  - `models/` — Trained model weights and metadata
  - `data/` — Raw and processed wafer datasets
  - `ui/` — Streamlit demo app
  - `tests/` — Unit and integration tests
  - `outputs/` — Generated heatmaps, predictions, explanations, and synthetic data
  - `reports/` — Generated PDF/HTML reports
  - `runlogs/` — Execution logs
- `docs/` — Overall system architecture and documentation

---

## Notes on Data and Models

This repository includes large assets under `server/data/`, `server/outputs/`, and `server/models/`.

If you plan to push to GitHub, consider using Git LFS or external storage for these files.
