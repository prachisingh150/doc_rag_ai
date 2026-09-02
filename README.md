# Complete RAG Chatbot Project Guide
 What The Project Does

This is a full-stack document chatbot.

It lets a user log in with a username, ask questions about files in `knowledge_base`, retrieve relevant document chunks using FAISS, answer with Gemini through LangChain, and save the chat history in PostgreSQL.

## Folder Map

- `backend/main.py` - FastAPI app. It loads the FAISS index, talks to Gemini, exposes API routes, and saves chat history.
- `backend/create_tables.py` - Creates the PostgreSQL tables for users and chat history.
- `backend/create_index.py` - Reads files from `knowledge_base` and creates the FAISS vector index.
- `backend/requirements.txt` - Python packages needed for both backend and frontend.
- `frontend/app.py` - Streamlit chat interface.
- `knowledge_base/` - Sample documents used by the chatbot.
- `faiss_index/` - Prebuilt vector index from the tutorial.
- `pip_installs.txt` - One-line package install command from the original project.

## Video Timeline Map

- `0:00` - Final app demo and project overview.
- `5:10` - Technology stack and folder setup.
- `16:21` - PostgreSQL database setup using `backend/create_tables.py`.
- `29:26` - Document ingestion and FAISS index creation using `backend/create_index.py`.
- `37:35` - Backend API and RAG logic in `backend/main.py`.
- `1:12:58` - Streamlit frontend in `frontend/app.py`.
- `1:39:07` - Full project demo and database check.

## Setup Steps

Open a terminal in this project folder.

1. Create and activate a Python environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

2. Install the packages:

```powershell
pip install -r backend\requirements.txt
```

3. Create the environment file:

```powershell
Copy-Item backend\.env.example backend\.env
```

4. Edit `backend/.env` and fill in your own values:

```text
GOOGLE_API_KEY=your_google_gemini_api_key_here
DATABASE_URL=postgresql://postgres:your_password@localhost:5432/rag_chatbot
```

5. Make sure PostgreSQL is running and the database in `DATABASE_URL` exists.

6. Create the database tables:

```powershell
cd backend
python create_tables.py
```

7. Rebuild the FAISS index if you change files in `knowledge_base`:

```powershell
python create_index.py
```

The extracted project already includes `faiss_index`, so this step is optional unless you change the documents.

8. Start the backend:

```powershell
uvicorn main:app --reload
```

Backend URL:

```text
http://127.0.0.1:8000
```

API docs:

```text
http://127.0.0.1:8000/docs
```

9. Open another terminal, activate the same Python environment, and start the frontend:

```powershell
cd frontend
streamlit run app.py
```

## How The Pieces Connect

The frontend sends your username to `/get_or_create_user`.

When you ask a question, the frontend sends it to `/query`.

The backend loads previous messages from PostgreSQL, retrieves document context from FAISS, asks Gemini for an answer through LangChain, saves the new question and answer in PostgreSQL, then sends the answer back to Streamlit.

## Main API Routes

- `GET /` - Basic welcome check.
- `POST /get_or_create_user` - Logs in or creates a username.
- `POST /get_history` - Loads old chat messages for a user.
- `POST /query` - Runs the RAG chatbot and saves the conversation.

## Important Notes

Run backend commands from inside the `backend` folder because the tutorial code uses paths like `../faiss_index` and `../knowledge_base`.

Do not share your real `.env` file. It contains your API key and database password.

