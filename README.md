GovAssist
=========
AI-powered grievance triage and routing demo with a FastAPI backend and Streamlit monitoring dashboard.

Project layout
--------------
- `backend/app`: FastAPI service that analyzes text, assigns department/urgency, stores complaints in SQLite, and auto-escalates overdue items.
- `backend/app/seed_data.py`: Seeds 100 sample complaints on first run if the database is empty.
- `frontend/streamlit_app.py`: Streamlit dashboard for viewing, filtering, and updating complaint status.
- `backend/app/sgcrs_demo.db`: SQLite database created automatically on first start.

Tech stack
----------
- FastAPI, Pydantic, SQLAlchemy
- SQLite for persistence
- Optional HuggingFace transformer; falls back to rule-based NLP if unavailable
- Streamlit + Plotly for dashboarding

Backend setup (Windows / PowerShell)
------------------------------------
```powershell
cd backend
python -m venv venv
venv\Scripts\activate
pip install --upgrade pip
# Full install (includes transformers + torch)
pip install -r requirements.txt
# Faster install (skip heavy deps):
# pip install fastapi uvicorn sqlalchemy pydantic faker streamlit plotly
```

Run the API
-----------
```powershell
cd backend
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
# Logs should include: Escalator thread started.
```

Frontend setup
--------------
```powershell
cd frontend
streamlit run streamlit_app.py
```

Key API endpoints
-----------------
- `POST /analyze` — Analyze text and create a complaint. Body: `{ "citizen_name": "Alice", "text": "Streetlight not working" }`.
- `GET /complaints` — List complaints.
- `GET /complaints/{id}` — Fetch one complaint.
- `POST /complaints/{id}/update_status` — Update status (`in_progress`, `resolved`, `escalated`).

Behavior notes
--------------
- Auto seed: On first API start, `seed_data.py` runs if no complaints exist, adding 100 sample records with varied departments/urgency/status.
- Escalation loop: A background thread escalates in-progress complaints older than 48 hours (adjust `ESCALATION_SECONDS` in `app.main`).
- Routing: Simple keyword-based classifier (`nlp.py`) chooses department and urgency; falls back gracefully when transformers are unavailable.
- Data: SQLite file lives at `backend/app/sgcrs_demo.db`.

Using the dashboard
-------------------
1) Start the backend, then launch Streamlit (default assumes API at `http://127.0.0.1:8000`).
2) Filter by department/status, view KPIs, urgent cases, and SLA breaches.
3) Update complaint status from the UI (calls the FastAPI endpoint).

Troubleshooting
---------------
- API not reachable: Confirm uvicorn is running and the `API_BASE` URL in `frontend/streamlit_app.py` matches your host/port.
- Slow install: Use the slim dependency set (skip transformers/torch) if you do not need the optional model.
- Fresh data: Delete `backend/app/sgcrs_demo.db` before restarting to reseed.
