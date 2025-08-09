# Multi-Modal Data Analysis API — README Template

**Project:** Multi-Modal Data Analysis API
**Description:** FastAPI-based REST API that orchestrates LLM workflows for multi-modal data analysis (text, CSV, images, code generation, etc.). This is a generic, shareable README template with **no personal data**.

---

## Quick overview

* REST API built with FastAPI
* Supports multi-file uploads; `questions.txt` (or similar) is required for analysis requests
* Multiple workflow types (data analysis, image analysis, code generation, EDA, predictive modeling, etc.)
* Synchronous processing for short-running tasks (service-level timeout / limits should be enforced separately)
* Designed to be run locally or in a container

---

## Highlights (v2.0)

* Multiple file upload support (1 required `questions.txt` + optional files)
* Intelligent workflow detection (configurable)
* Multi-modal processing (text, images, structured files)
* Enhanced logging and error handling
* Prebuilt workflows for common analytical tasks

---

## Requirements

* Python 3.10+
* Recommended: virtual environment (venv / conda)
* See `requirements.txt` or `pyproject.toml` for full dependency list

---

## Environment variables

Create a `.env` from the template `.env.template` and set required values. Example environment variables (do **not** commit secrets):

* `OPENAI_API_KEY` — API key for LLM provider (if used)
* `LANGCHAIN_TRACING_V2` — enable tracing (optional)
* `LANGCHAIN_API_KEY` — LangChain tracing key (optional)
* Other provider-specific keys as required by your configuration

---

## API Endpoints (example)

* `POST /api/` — Submit analysis tasks (multipart/form-data; `questions_txt` required)
* `GET /health` — Health check (returns 200 OK if service is healthy)
* `GET /` — API info / landing page

### Additional (optional) endpoints

* `POST /api/analyze` — JSON-based analysis request
* `POST /api/workflow` — Execute a specific workflow by name
* `POST /api/pipeline` — Execute a multi-step pipeline
* `GET /api/tasks/{id}/status` — Check task status

---

## Example usage

### Curl — multi-file upload

```bash
curl "http://localhost:8000/api/" \
  -F "questions_txt=@questions.txt" \
  -F "files=@dataset.csv" \
  -F "files=@image.png"
```

### Python — basic request

```python
import requests

resp = requests.post(
    "http://localhost:8000/api/analyze",
    json={
        "task_description": "Analyze customer churn data",
        "workflow_type": "data_analysis",
        "dataset_info": {
            "description": "Customer dataset",
            "columns": ["age", "tenure", "charges", "churn"],
            "sample_size": 1000
        }
    }
)
print(resp.json())
```

---

## Example workflows (configurable)

* `data_analysis` — General analysis & recommendations
* `image_analysis` — Image processing / CV pipelines
* `text_analysis` — NLP & text analytics
* `code_generation` — Generate executable Python code for analysis
* `exploratory_data_analysis` — EDA plan & execution
* `predictive_modeling` — Model guidance & training suggestions
* `data_visualization` — Chart/plot suggestions or generation
* `statistical_analysis` — Statistical tests & inference
* `web_scraping` — Web data extraction guidance
* `database_analysis` — SQL / DuckDB assistance

---

## Local development (quick start)

1. Clone the repository

```bash
git clone <REPO_URL>
cd <REPO_DIR>
```

2. Create & activate a virtual environment

```bash
python -m venv .venv
source .venv/bin/activate   # Linux/macOS
.venv\Scripts\activate      # Windows (PowerShell)
```

3. Install dependencies

```bash
pip install -r requirements.txt
```

4. Create `.env` from the template and set variables

```bash
cp .env.template .env
# edit .env to add required keys (do not commit)
```

5. Run the server (development)

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

6. Visit the API docs

```
http://localhost:8000/docs
```

---

## Docker (optional)

Build and run using the provided scripts or manually:

```bash
# build
docker build -t data-analysis-api .

# run (example)
docker run -d --name data-analysis-api -p 8000:80 --env-file .env data-analysis-api
```

---

## Tests

* Simple tests can be run with included test scripts:

```bash
python test_api.py
python test_file_upload_api.py
```

(Adapt names to your test files)

---



