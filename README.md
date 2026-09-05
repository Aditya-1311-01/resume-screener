# Resume Screener

Everything lives in a single folder now:

```
resume-screener/
  main.py            <- backend (FastAPI, wraps your original script)
  index.html          <- frontend (open directly in a browser)
  requirements.txt
  .env.example
```

## Step-by-step procedure

### 1. Open the folder in VS Code
`File → Open Folder…` → select `resume-screener`. You should see all four files in the Explorer sidebar.

### 2. Open a terminal
`` Ctrl + ` `` (backtick) opens the integrated terminal at the bottom, already inside the `resume-screener` folder.

### 3. Create a virtual environment
```bash
python -m venv venv
```

Activate it:
```bash
venv\Scripts\activate
```
(macOS/Linux: `source venv/bin/activate`)

Your terminal prompt should now start with `(venv)`.

### 4. Install dependencies
```bash
pip install -r requirements.txt
```

### 5. Add your API key
Rename `.env.example` to `.env` (right-click the file in VS Code → Rename), then open it and replace the placeholder:
```
GROQ_API_KEY=gsk_your_real_key_here
```

### 6. Start the backend
```bash
uvicorn main:app --reload --port 8000
```
Leave this terminal running — you'll see `Uvicorn running on http://127.0.0.1:8000`. Check it worked by opening `http://localhost:8000/api/health` in a browser; it should show `{"status":"ok"}`.

### 7. Open the frontend
Right-click `index.html` in VS Code → **Open with Live Server** (install the "Live Server" extension first if you don't have it — Extensions icon on the left sidebar, search "Live Server", Install).

No Live Server? Just double-click `index.html` in your file explorer — it opens fine directly in a browser too, since it makes no assumptions about how it's served.

### 8. Use it
1. Paste a job description into the left panel.
2. Drop in PDF/DOCX resumes.
3. Click **Screen candidates**.
4. Ranked results appear on the right.

**Keep the terminal from step 6 running the whole time** — the page in your browser calls `localhost:8000`, so if that server stops, the page will show a connection error.

## If something breaks

| Symptom | Likely cause |
|---|---|
| "Could not reach the backend" in the browser | `uvicorn` isn't running, or you closed that terminal |
| `ModuleNotFoundError` when starting uvicorn | venv isn't activated, or step 4 wasn't run in it |
| `API key kaha hai bhai` error on startup | `.env` is missing or still has the placeholder key |
| A resume shows as an error row | It's likely a scanned/image-only PDF with no selectable text |
| Groq rate-limit errors with many resumes | Screen resumes in smaller batches, or add a short delay between them in `main.py`'s loop |
