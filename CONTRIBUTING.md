# Contributing

Start by reading `README.md`, `docs/START_LOCAL.md`, and `docs/HISTORY_LOCAL.md`. Keep medical screening outside the application boundary and avoid adding real personal or medical data to tests.

Before submitting a change, run the relevant checks:

```bash
cd lifelink_fastapi
python3 -m compileall -q app tests
pytest -q

cd ../LifeLinkAndroid
./gradlew testDebugUnitTest lintDebug assembleDebug
```

Explain behavioral changes in the pull request, add regression coverage for API changes, and update the relevant changelog or audit documentation when a user-facing or security-relevant behavior changes.
