To Create the python environment in the backend folder

To setup the location
cd backend

Create the python environment
python -m venv venv

To activate the python environment
venv\Scripts\activate

To upgrade the pip packages
pip install --upgrade pip

TO download the requirements in the backend folder
pip install -r requirements.txt

# If you want faster install and avoid heavy libs:
# remove transformers & torch from backend/requirements.txt then:
# pip install fastapi uvicorn sqlalchemy pydantic faker streamlit plotly

**For the Backend Running:**

To set the backend path in command prompt:
cd backend

To run the backend code:
python -m uvicorn app.main:app --reload --host 0.0.0.0 --port 8000

""For the Frontend Running:**

To set the frontend path in command prompt:
cd frontend

To run the Frontend code:
python -m streamlit run streamlit_app.py