# LifeLink Local Package

This package runs the FastAPI demo locally with in-memory storage. It does not require a cloud account, PostgreSQL, or payment.

## Start the API

```bash
cd lifelink_fastapi
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

Open `http://localhost:8000/docs`.

## Android emulator

Use `http://10.0.2.2:8000/` as the API base URL. For a physical phone on the same Wi-Fi network, use the computer's local IP address, such as `http://192.168.1.25:8000/`.

Data in demo mode is held in memory and resets when the API stops. Use the separate LifeLink Cloud package when you need PostgreSQL, Supabase, PostGIS, or public Render deployment files.

## Included

- Native Android source
- FastAPI demo backend
- Expo mobile source
- Tests and documentation
- Launcher icon
- Debug APK
