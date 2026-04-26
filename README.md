# Medicine Tracker — Deployment Guide

This repository is a Flask app using SQLAlchemy and optional Twilio SMS integration.

Quick checklist before deploying:
- Create a production `SECRET_KEY` and set `DATABASE_URL` pointing to a MySQL instance.
- If you want real SMS, set Twilio env vars: `TWILIO_SID`, `TWILIO_AUTH_TOKEN`, `TWILIO_PHONE_NUMBER`.

Options to deploy

1) Docker (recommended for predictable runtime)

PowerShell commands:

```powershell
# Build image
docker build -t medicine-tracker .

# Run container (replace env values)
docker run -e SECRET_KEY="REPLACE" -e DATABASE_URL="mysql+pymysql://user:pass@host:3306/db" -p 8080:8080 medicine-tracker
```

2) Platform-as-a-Service (Render / Railway / Heroku)

- Ensure `requirements.txt` contains `gunicorn` (already included).
- Add your env vars in the service dashboard:
  - `SECRET_KEY`, `DATABASE_URL`, `TWILIO_SID`, `TWILIO_AUTH_TOKEN`, `TWILIO_PHONE_NUMBER`.
- Procfile is provided for Heroku-like platforms; the start command is `gunicorn app:app`.

3) Azure App Service

- Use Docker image or use direct Python deployment. Set the same environment variables in App Settings.

Database migrations and initial setup

The app auto-runs `db.create_all()` when started under `__main__`. For production, consider using Flask-Migrate and running SQL migrations instead of `create_all()`.

Security notes

- Do not store secrets in source code. Use platform secret stores or env vars.
- Change default `SECRET_KEY` before going to production.

Troubleshooting

- If you see DB connection errors, verify the `DATABASE_URL` syntax and network access to the MySQL host.
- For Twilio errors, check the Twilio credentials and that the `from` phone number is a Twilio number.

