# LifeLink Local FastAPI Demo

This folder contains the local in-memory LifeLink API. It is designed for learning, local Android testing, and demonstrations. It does not require PostgreSQL, Supabase, Render, or a payment method.

## Start

```bash
cd lifelink_fastapi
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

Open `http://localhost:8000/docs` for the interactive API documentation.

The Android emulator uses `http://10.0.2.2:8000/`. A physical phone on the same Wi-Fi network should use the computer’s local IP address.

## Limitation

The demo stores records in memory. Data resets when the API stops. Use the separate LifeLink Cloud package for PostgreSQL, Supabase, PostGIS, Docker, and Render deployment.
