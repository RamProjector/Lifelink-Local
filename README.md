# LifeLink Local

LifeLink Local is the no-cloud development package for the LifeLink emergency blood-donor discovery helper. It contains the native Kotlin/Jetpack Compose Android source, a FastAPI in-memory demo backend, the Expo mobile source, tests, documentation, and a development APK.

## What LifeLink does

A requester creates an emergency blood request, reviews GPS-assisted donor matches, selects donors, and requests contact. Donors can maintain their profile and availability and respond to eligible requests. Medical screening remains outside LifeLink and must be handled by the responsible clinical or blood-bank process.

## Quick start

Start the local API:

```bash
cd lifelink_fastapi
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

The interactive API documentation is available at `http://localhost:8000/docs`. The Android emulator reaches this service at `http://10.0.2.2:8000/`; a physical phone should use the computer’s LAN IP address.

Build the Android source:

```bash
cd LifeLinkAndroid
./gradlew testDebugUnitTest lintDebug assembleDebug
```

The local development APK is in `artifacts/app-debug-local.apk`.

## Package structure

| Path | Purpose |
|---|---|
| `LifeLinkAndroid/` | Native Kotlin/Compose Android application |
| `lifelink_fastapi/` | Local in-memory FastAPI service |
| `lifelink-mobile/` | Expo/React Native mobile source |
| `docs/` | Startup guide, changelog, project history, and quality audit |
| `artifacts/` | Development APK and app icon |

## Important limitation

The local API stores records in memory. Data resets when the server stops. PostgreSQL, Supabase, PostGIS, Docker, and Render deployment files are in the separate [LifeLink cloud repository](https://github.com/RamProjector/LifeLink).

## Documentation

Read [`docs/START_LOCAL.md`](docs/START_LOCAL.md) first. The concise milestone record is [`docs/CHANGELOG_LOCAL.md`](docs/CHANGELOG_LOCAL.md), and the student-oriented explanation is [`docs/HISTORY_LOCAL.md`](docs/HISTORY_LOCAL.md).

## Security note

This package is for local development. Do not use the demo authentication seam, in-memory storage, or development API URL as a production security design. See [`SECURITY.md`](SECURITY.md).

## License

Released under the MIT License. See [`LICENSE`](LICENSE).
