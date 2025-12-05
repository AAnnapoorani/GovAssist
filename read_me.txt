cd backend
python -m venv venv
venv\Scripts\activate
pip install --upgrade pip
# If you want the transformer pipeline and have internet, install all:
pip install -r requirements.txt
# If you want faster install and avoid heavy libs:
# remove transformers & torch from backend/requirements.txt then:
# pip install fastapi uvicorn sqlalchemy pydantic faker streamlit plotly



cd backend
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
# You should see "Escalator thread started." printed.


cd frontend
streamlit run streamlit_app.py
