# LifeLink Local Package — Student Project History

This document explains the LifeLink project from the beginning in teaching language. It describes the major milestones, why they were needed, how the parts connect, and what a student can learn from each stage. It is a project history, not a complete Git commit log.

## 1. The original problem

LifeLink was designed as a mobile helper for emergency blood-donor discovery. A coordinator creates an emergency request, the system finds nearby available donors, and the coordinator chooses which donors to contact.

LifeLink does not replace hospitals, doctors, blood banks, or medical screening. Its responsibility is operational: discovery, matching support, communication requests, and status tracking.

## 2. Users and journeys

The application has two main roles:

- **Requester/coordinator:** creates a request, reviews donors, selects donors, and contacts them.
- **Donor:** creates a profile, sets availability, views eligible requests, and responds.

This role separation influenced the screen structure, domain models, API routes, and authorization checks.

## 3. Visual planning

The app was planned with a modern Material 3 direction: cards, clear spacing, strong primary actions, status colors, confirmation dialogs, progress feedback, and readable explanations.

A key design lesson is that screens should be planned around journeys. A visually attractive screen is not enough if a user cannot complete the task or if a visible button does nothing.

## 4. Native Android foundation

The main app was implemented in Kotlin with Jetpack Compose. Compose renders the interface from state, while ViewModels and repositories manage behavior and data.

The project gained Gradle configuration, Android metadata, Material 3 theming, reusable components, test tasks, lint tasks, and debug APK packaging.

The basic architecture is:

```text
Compose UI → ViewModel → Repository → Retrofit API → FastAPI
```

## 5. Emergency request flow

The requester flow became a multi-step process collecting blood type, units, urgency, facility, location, deadline, contact method, note, and consent confirmations.

The client validates fields for immediate feedback. The server validates them again because a client can be modified or bypassed. Critical requests include an additional confirmation step to reduce accidental emergency broadcasts.

## 6. FastAPI demo backend

The first backend used FastAPI and in-memory storage through `app.main:app`. FastAPI provides typed request validation, API routes, interactive `/docs`, and a clear path toward a production service.

The demo mode is intentionally easy to run. Its limitation is that data disappears when the server stops.

## 7. GPS-assisted matching

Donors are filtered and ranked using compatibility, availability, verification, geographic distance, estimated travel time, availability freshness, response likelihood, and urgency.

The score is an operational priority, not a medical eligibility decision. Explanations are returned so the requester can understand the ranking.

An optional AI-matching toggle was added. When disabled, the predictable rule-based path remains available. This provides a fallback for explainability, cost, reliability, and testing.

## 8. Donor mode

The donor experience gained profile editing, service area, availability controls, an inbox of nearby eligible requests, accept and decline actions, and response-state display.

The two-sided workflow matters because a donor discovery app must allow donors to communicate their current availability.

## 9. Android networking and state

Retrofit contracts, domain models, repositories, and ViewModels were added. A normal submission sequence is:

1. The user edits fields.
2. Compose updates state.
3. The user submits.
4. The repository calls FastAPI.
5. The ViewModel exposes loading, success, or error state.
6. Compose redraws the correct result.

This keeps screens focused on presentation and makes behavior testable.

## 10. Offline reliability

Room was added for local drafts and status caching. WorkManager was added for retrying operations after temporary network failures.

Idempotency keys help prevent duplicate requests when a retry occurs. This teaches that mobile reliability must account for disconnected networks, process restarts, and repeated taps.

## 11. PostgreSQL-ready structure

The project gained SQLAlchemy models, asynchronous database sessions, repositories, and a PostgreSQL adapter. The local package still defaults to the simpler demo backend, but the persistent implementation is included for comparison and future use.

The schema includes facilities, donors, emergency requests, request matches, and pending submissions. Constraints protect units, coordinates, statuses, consent, and idempotency.

## 12. Requester-selected donor contact

After matching, requesters can select specific donor cards instead of contacting every match. Cards show relevant context such as blood type, distance, travel time, and ranking explanation.

The contact endpoint is:

```text
POST /v1/emergency-requests/{request_id}/contact
```

The server verifies that the request exists, each selected donor is an eligible match, and the requester owns the request when authentication is enabled.

## 13. Security seams

Bearer-token seams and ownership checks were added. The current token behavior is useful for development tests but is not a finished identity provider. A public deployment should use verified Firebase, Supabase, or equivalent JWT authentication.

The Android app should never receive the PostgreSQL password. It should call FastAPI instead.

## 14. Audit milestones

### Audit5 — Dual-mode discovery

Verified that the updated product idea was represented: GPS-assisted donor discovery, optional AI ranking, requester selection, and contact before external screening. Added regression tests for successful contact and invalid donor selection.

### Audit6 — Icon integration

Designed the LifeLink icon using a location pin, heart, and medical cross. Connected it to the Android manifest and verified that it was embedded in the APK. Corrected stale tooling evidence in the audit report.

### Audit7 — Interaction and authorization

Found an empty click handler on an informational urgency chip. Replaced it with a non-interactive status surface. Added a test preventing one authenticated coordinator from contacting donors for another coordinator’s request. The empty-handler scan became clean.

## 15. Local package

The local package is designed for learning and development. It requires no cloud account and no PostgreSQL installation.

```bash
cd lifelink_fastapi
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

The Android emulator uses `http://10.0.2.2:8000/`. A physical phone on the same Wi-Fi network uses the computer’s local IP address.

## 16. Remaining production work

A public launch would still require verified authentication, notification delivery, privacy review, data retention rules, rate limiting, monitoring, production migration management, backups, and formal review against local blood-bank policy.

## Learning summary

The project grew in layers:

```text
Problem definition
→ user journeys
→ Android UI
→ FastAPI prototype
→ donor matching
→ donor mode
→ persistence and offline behavior
→ requester-controlled contact
→ authorization
→ audit and packaging
```

Each layer answered a product or engineering problem discovered by the previous layer.

## Audit8 — Build configuration lesson

The Android API URL was made configurable at build time. A student can now build the same app against the emulator demo, a phone-accessible LAN server, or a public HTTPS API using a Gradle property or environment variable. This avoids changing source code for every deployment target and reduces the chance of accidentally shipping a development URL.

## Audit9 — Release and security defaults

A release audit compared the Android and FastAPI route inventories and verified that the client contract has matching backend paths. The PostgreSQL adapter was changed to fail closed when authentication configuration is missing, while the local in-memory demo remains easy to run anonymously. Android debug and release builds were verified with a configurable HTTPS endpoint.

## Audit10 — Repository hygiene and CI security

The GitHub repository was reviewed like a shared software project rather than only a source archive. Historical build logs were removed because they add noise and can accidentally reveal environment details. The CI workflow was restricted to read-only repository contents and configured to cancel duplicate runs, reducing unnecessary work and limiting workflow permissions.
