# GovAssist

Smart Grievance Classification & Routing prototype with a FastAPI backend and a Streamlit dashboard frontend.

## Project layout
- backend/app: FastAPI service, SQLite DB, NLP/routing logic
- backend/requirements.txt: backend deps (transformer/torch optional)
- frontend/streamlit_app.py: dashboard UI

## Prerequisites
- Python 3.11
- Windows PowerShell commands shown; adapt activation for other shells/OS

## Backend setup
```powershell
cd backend
python -m venv venv
venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt
```

### Optional: lighter install
If you want to skip heavy ML deps, remove `transformers` and `torch` from `backend/requirements.txt` and install the rest:
```powershell
pip install fastapi uvicorn sqlalchemy pydantic faker streamlit plotly
```
The app will fall back to rule-based NLP when transformers are absent. To explicitly enable the transformer path (requires compatible torch/numpy), set `USE_TRANSFORMER=1` before starting the server.

### Run the API
```powershell
cd backend
python -m uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```
You should see "Escalator thread started." when it’s up.

### Seed sample data (optional)
In a separate shell with the venv active:
```powershell
cd backend
python -m app.seed_data
```

## Frontend setup
In another shell:
```powershell
cd frontend
python -m streamlit run streamlit_app.py
```

## API surface (summary)
- POST /analyze – analyze text, auto-route, store complaint
- GET /complaints – list complaints
- GET /complaints/{id} – fetch one complaint
- POST /complaints/{id}/update_status – change status (in_progress/resolved/escalated)

## Notes
- DB is SQLite at backend/sgcrs_demo.db. Delete it to start fresh.
- Background escalator auto-escalates in-progress items past the SLA window.
- Keep backend running before launching the Streamlit dashboard.
