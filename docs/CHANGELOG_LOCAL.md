# LifeLink Local Package Changelog

This changelog lists the major release milestones. For the teaching-style explanation of what changed, why it changed, and how the parts connect, read [HISTORY_LOCAL.md](HISTORY_LOCAL.md).

## Project foundation

- Defined LifeLink as a GPS-assisted blood-donor discovery helper.
- Established requester/coordinator and donor roles.
- Planned modern Material 3 mobile screens and complete emergency-request journeys.

## Native Android MVP

- Built the Kotlin/Jetpack Compose Android application.
- Added Material 3 theming, reusable cards, navigation, ViewModels, repositories, and Retrofit contracts.
- Implemented the multi-step emergency-request flow with validation, consent, critical-request confirmation, status polling, and cancellation.
- Added donor profile, availability, inbox, accept, and decline flows.

## Backend and matching

- Added the FastAPI in-memory demo backend at `app.main:app`.
- Added GPS-assisted donor matching with compatibility, distance, travel-time, freshness, verification, response-likelihood, and urgency factors.
- Added explainable ranked results and an optional AI-ranking toggle.
- Added manual broadcast and request status behavior.

## Reliability and persistence

- Added Room for local drafts and cached status.
- Added WorkManager retry behavior for temporary network failures.
- Added idempotency protections.
- Added PostgreSQL/SQLAlchemy models, repositories, schema, and route adapter for persistent deployments.

## Audit5 — Dual-mode discovery

- Added requester-selectable donor cards.
- Added selected-donor contact action and external-screening disclosure.
- Added `POST /v1/emergency-requests/{request_id}/contact`.
- Added matched-donor validation, ownership checks, and regression tests.

## Audit6 — App icon and packaging

- Designed and integrated the LifeLink launcher icon.
- Wired `android:icon` and `android:roundIcon`.
- Verified the icon inside the APK.
- Corrected stale tooling evidence in the audit report.

## Audit7 — Interaction and authorization

- Replaced an empty urgency-chip click handler with a non-interactive status surface.
- Added authenticated cross-owner contact regression coverage.
- Reached 14 passing backend tests and a clean empty-handler scan.

## Local package split — 2026-09-15

- Created a standalone package for local development without cloud accounts or PostgreSQL.
- Included Android source, Expo source, FastAPI demo source, tests, instructions, audit report, APK, and icon.

## Local behavior

The local package runs `app.main:app` with in-memory storage. Data resets when the server stops. See `docs/START_LOCAL.md` for setup and `docs/HISTORY_LOCAL.md` for the detailed teaching history.

## Audit8 — Build configuration and cloud handoff — 2026-09-16

Made the Android API base URL configurable with `-PlifelinkApiBaseUrl=...` or `LIFELINK_API_BASE_URL`, while retaining the emulator default. Corrected the Android README’s Gradle-wrapper statement. Backend verification now passes 15 tests.

## Audit9 — Release and security defaults — 2026-09-16

Compared Android and FastAPI route inventories, confirmed package hygiene, and verified Android debug/release builds with configurable API endpoints. The PostgreSQL adapter now defaults to authentication required when its environment setting is omitted; local demo behavior remains unchanged. Backend verification remains at 15 passing tests.
