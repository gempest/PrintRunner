# PrintRunner

PrintRunner is a student project for campus printing and delivery. The website uses Flask and SQLite for orders, runner applications, and the staff order board. PyScript powers the browser-side print estimate.

## Run locally

Requirements: Python 3.10 or later. In PowerShell, from this folder:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python app.py
```

Open <http://127.0.0.1:8000>. PyScript and web fonts load from external CDNs, so the browser needs internet access for those resources.

The local SQLite database and uploaded print files are stored under `instance/`. That folder is excluded from Git. Print uploads are limited to 15 MB and accepted file types are PDF, Office documents, and images.

## Create the first staff account

In a second PowerShell window, activate the same virtual environment and run:

```powershell
python app.py create-staff runner
```

Enter a unique password of at least 12 characters when prompted. Staff accounts are created by the project administrator; there is no public staff registration page.

## Main features

- Customers can estimate an order, submit print details, and optionally upload a file.
- Runner applications are saved for the team to review.
- Signed-in service staff can view orders, download attached print files, and update order statuses.
- The customer homepage includes the PrintRunner commercial in `media/`.

## Deploy on Railway

1. Connect the `gempest/PrintRunner-Final` repository and deploy its `main` branch.
2. Set the start command to `gunicorn app:app --bind 0.0.0.0:$PORT --workers 1`.
3. Attach a persistent volume mounted at `/data` and set `PRINTRUNNER_DATA_DIR=/data`.
4. Set `PRINTRUNNER_COOKIE_SECURE=1` for HTTPS. The app generates a stable session secret in its data directory unless `PRINTRUNNER_SECRET_KEY` is set explicitly.
5. After the first deploy, create a staff account from the Railway service shell with `python app.py create-staff runner`.
6. Generate a Railway public domain and open `/staff.html` to sign in.

GitHub Pages cannot run the Flask backend. Keep `instance/`, secrets, credentials, and customer uploads out of Git. This is an educational project and needs a privacy/security review before use with real customer data.
