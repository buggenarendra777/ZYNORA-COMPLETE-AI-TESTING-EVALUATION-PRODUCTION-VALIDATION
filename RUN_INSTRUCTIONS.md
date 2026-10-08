# Zynora — Presentation Run Guide

## Fastest frontend demo

```bash
pnpm install
pnpm run build
pnpm run dev
```

Open the Vite URL shown in the terminal. Sign in with `demo@zyngram.com` and any password with 6+ characters. The app uses the local approved-knowledge adapter by default, so no API key is needed.

## Optional Flask API

Terminal 1:

```bash
python3 -m venv .venv
. .venv/bin/activate
pip install -r backend/requirements.txt
export DATABASE_URL=sqlite:///zynora-local.db
python -m backend.seed_local
python -m backend.app
```

Terminal 2:

```bash
export VITE_API_BASE_URL=http://127.0.0.1:5000
pnpm run dev
```

Health check: `curl http://127.0.0.1:5000/api/health`

## Presentation flow

1. Login → use the help assistant link to demonstrate password support.
2. Ask `What is Zyngram?` and open evidence.
3. Ask `What is the physical franchise hierarchy?`, then `What comes after Hub?`.
4. Click **New**, **Clear**, and **Delete** to demonstrate conversation management.
5. Ask an unknown question and an injection question; verify the exact fallback.
6. Open **Monitoring** for Day 12/13 validation evidence.
7. Open **Knowledge admin** to demonstrate approved/active controls.

## Reset demo state

In browser DevTools Console: `Object.keys(localStorage).filter(k => k.startsWith('zynora-')).forEach(k => localStorage.removeItem(k)); location.reload()`

## Day 13 validation commands

From the project root:

```bash
python3 -m venv .venv
. .venv/bin/activate
pip install -r backend/requirements.txt
pnpm install --frozen-lockfile
rm -f evaluation.db
.venv/bin/python scripts/evaluate_40.py
.venv/bin/python scripts/performance_test.py
pnpm run typecheck
pnpm run build
```

The evaluation runner updates `TEST_DATASET_40.csv` and should finish with `total=40 pass=40 review=0 fail=0`. The backend exposes `/api/observability/summary` for telemetry verification.
