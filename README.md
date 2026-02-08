# SHL Assessment Recommender System

[**Live Demo**](https://drive.google.com/file/d/1Ba1Wx1xPJS7L92aoiVvLD9v1gezym8bQ/view?usp=drive_link)

An **AI-powered recommendation system** that suggests the most relevant **SHL assessments** based on natural language hiring queries. The system combines **lexical search (BM25)**, **semantic search (FAISS + embeddings)**, and **intelligent ranking logic** to deliver accurate, balanced, and explainable recommendations.

## Key Features

- **Natural Language Query Understanding**
- **Hybrid Retrieval Engine**: BM25 (lexical relevance) & Dense embeddings + FAISS
- **Ensemble Ranking**: Weighted scoring from multiple retrieval strategies
- **Balanced Recommendations**: Ensures a mix of Knowledge (K) and Personality (P) tests
- **FastAPI-based Backend**
- **Streamlit Frontend UI**
- **Health Monitoring Endpoint**
- Modular, extensible, and production-ready design

## System Architecture

```
User Query
│
▼
Streamlit Frontend
│
▼
FastAPI Backend
│
├── BM25 Search (Lexical)
├── FAISS Search (Semantic)
├── Keyword Matching
├── Test-Type Heuristics
│
▼
Ensemble Scoring & Balancing
│
▼
Top-K SHL Assessment Recommendations
```

## > Project Structure

```
SHL_Assessment_Recommender/
│
├── app.py                     # FastAPI backend
├── streamlit_ui.py             # Streamlit frontend
├── requirements.txt
├── README.md
│
├── data/
│   ├── products.csv / parquet
│   ├── index/
│   │   ├── index.faiss
│   │   ├── embeddings.npy
│   │   ├── bm25.pkl
│
├── models/
│   └── crossencoder-retrained/
│
└── .env
````

## Tech Stack

| Layer | Technology |
|-----|-----------|
| Backend | FastAPI |
| Frontend | Streamlit |
| Search | BM25, FAISS |
| Embeddings | Sentence Transformers / External APIs |
| Data | Pandas, NumPy |
| Deployment | Render, Streamlit Cloud |

## Installation & Local Setup

### 1️) Clone the Repository

```bash
git clone https://github.com/<your-username>/SHL-Assessment-Recommender.git
cd SHL-Assessment-Recommender
````

### 2️) Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate      # Linux/Mac
venv\Scripts\activate         # Windows
```

### 3️) Install Dependencies

```bash
pip install -r requirements.txt
```

### 4️) Run Backend API

```bash
uvicorn app:app --reload --host 0.0.0.0 --port 8000
```

API will be available at:

```
http://localhost:8000
```

### 5️) Run Frontend

```bash
streamlit run streamlit_ui.py
```

Frontend will be available at:

```
http://localhost:8501
```

## API Endpoints

### 1. Health Check

```http
GET /health
```

**Response**

```json
{
  "status": "healthy",
  "products_loaded": 388,
  "faiss_index_loaded": true,
  "bm25_loaded": true
}
```

### 2. Recommendation API

```http
POST /recommend
```

**Request Body**

```json
{
  "query": "Senior Python backend engineer",
  "k": 10
}
```

**Response**

```json
{
  "query": "Senior Python backend engineer",
  "results": [
    {
      "assessment_name": "Python Programming Test",
      "url": "https://www.shl.com/...",
      "test_type": "K",
      "duration": "40",
      "score": 0.87
    }
  ]
}
```

## Recommendation Logic (High Level)

- **BM25** finds keyword matches
- **FAISS + Embeddings** finds semantic matches
- **Keyword filters** improve recall
- **Test-type heuristics** enforce balance
- **Weighted ensemble scoring** ranks candidates
- **Final balancing layer** selects top-K results

## Environment Variables

```env
DEFAULT_API_URL=http://localhost:8000
RECOMMEND_API=http://localhost:8000/recommend
HEALTH_API=http://localhost:8000/health
```

(Optional keys for future LLM-based extensions can also be added.)
