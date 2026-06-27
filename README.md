# AutoYield-AI

AutoYield-AI is an end-to-end autonomous wafer quality inspection system. It combines computer vision defect detection, model explainability, drift monitoring, synthetic data generation, and optional GenAI-assisted root-cause analysis in a unified monorepo.

## What AutoYield-AI Does

- Classifies wafer defects using an EfficientNet-B0 image model.
- Creates Grad-CAM heatmaps for model explainability.
- Generates root-cause summaries using deterministic rules or Google Gemini when configured.
- Monitors drift through confidence-based metrics and triggers synthetic data generation.
- Provides both a React/Vite operations dashboard and a Streamlit demo interface.

## Tech Stack

- Backend: FastAPI, PyTorch, torchvision, OpenCV, NumPy, scikit-learn
- Frontend: React, Vite, React Router
- Optional GenAI: Google Gemini via `google-generativeai`

## Repository Layout

```
AutoYeildAI/
├── client/      Frontend dashboard (React + Vite)
├── server/      Backend application, ML pipelines, demo UI, scripts
└── docs/        Architecture and project documentation
```

## Getting Started

### 1) Run the Backend API

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

### 2) Start the Web Dashboard

```bash
cd client
npm install
npm run dev
```

The dashboard expects the backend API to be available at `http://localhost:8000`.

### 3) Launch the Streamlit Demo

```bash
cd server
streamlit run ui/dashboard.py
```

## Configuration

### Backend environment variables (`server/.env`)

```env
MONGO_URI=mongodb+srv://USER:PASSWORD@cluster0.0dwbwfe.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0
MONGO_DB_NAME=autoyield
MONGO_SERVER_SELECTION_TIMEOUT_MS=5000
CORS_ORIGINS=http://localhost:5173,http://127.0.0.1:5173
```

> If your MongoDB password contains special characters such as `@`, `:`, `/`, `?`, `#`, or `%`, URL-encode it.

### Frontend environment variables (`client/.env`)

```env
VITE_API_BASE_URL=http://localhost:8000
```

### Optional Gemini configuration

```env
GEMINI_API_KEY=your_key_here
GEMINI_MODEL=gemini-1.5-flash
```

If Gemini is not configured, the app falls back to deterministic root-cause analysis rules.

## Project Structure

- `client/` — React dashboard and UI code
- `server/api/` — FastAPI service and REST endpoints
- `server/src/` — core ML and pipeline implementation
- `server/config/` — YAML configuration and prompt templates
- `server/models/` — trained model weights and metadata
- `server/data/` — raw and processed wafer datasets
- `server/ui/` — Streamlit demo app
- `server/scripts/` — inference, retraining, drift, and RAG utilities
- `server/tests/` — unit and integration tests
- `server/outputs/` — generated heatmaps, predictions, explanations, and synthetic data
- `server/reports/` — generated reports and inspection summaries
- `server/runlogs/` — execution logs and diagnostic outputs

## Notes

- The repository contains large model and dataset assets under `server/data/`, `server/models/`, and `server/outputs/`.
- For GitHub publishing, use Git LFS or external storage for large files.
- The system is designed to support both local development and deployment-ready API/dashboard workflows.
