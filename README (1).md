# SkillSync

**A swipe-first, explainable job-matching prototype.** A Flutter app (Android, plus a web build
for classroom demos) and a Python FastAPI backend with Firebase/Adzuna adapters. The app also runs
completely **offline** with a fictional candidate and fictional jobs, so a demo needs no server,
account or internet connection.

| Read this | For |
| --- | --- |
| [docs/DEMO_SCRIPT.md](docs/DEMO_SCRIPT.md) | A timed eight-minute demo, fallbacks and viva questions |
| [docs/RUBRIC_MAP.md](docs/RUBRIC_MAP.md) | The rubric bands, evidence, research templates and objectives |
| [docs/CONTRIBUTION_WORKSHEET.md](docs/CONTRIBUTION_WORKSHEET.md) | Each student's own evidence (blank on purpose) |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Product flow, schema, API, matching algorithm and tool choices |
| [docs/PLAY_STORE_READINESS.md](docs/PLAY_STORE_READINESS.md) | What is done, and what the team must do to publish |
| [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md), [docs/API_EXAMPLES.md](docs/API_EXAMPLES.md) | Connected setup, build notes and API examples |

## This delivery

This ZIP contains the source code, tests and documentation. It is **not a deployed
service**, and the debug APK that ships beside it is for demonstration only.

**Verified before delivery**

- Backend regression tests: 28 passed. Flutter analysis: no issues. Flutter tests: 60 passed.
- The web build was driven through a real browser: Discover to apply to receipt to Tracker;
  an external apply that the student confirms as self-reported; the profile validation guard;
  and a learning quiz.
- A debug APK (`dev.skillsync.skillsync`, version 2.0.0) was built, inspected and signature
  checked. **It has not been run on a physical phone**, so do that before you present.

**Not done, and not claimed**

- Firebase, Adzuna and Google Play Billing are not configured, and no real partner
  credentials are included. The release signing key, privacy policy and Play Console
  steps belong to the team (see [docs/PLAY_STORE_READINESS.md](docs/PLAY_STORE_READINESS.md)).
- The contribution worksheet, primary research and viva answers are the students' own work
  and are left blank on purpose.

Extract the ZIP, then upload the **contents of its `SkillSync` folder** to your
GitHub repository; uploading only the ZIP does not expose the source as repository
files. Include hidden `.github` and `.gitignore` entries. Team members still need
their own genuine commits before the grading deadline.

## What is implemented

- **Offline showcase (the default entry):** a fictional candidate, 14 fictional jobs, 6 learning
  modules with quizzes, saved roles, a tracker, notifications and local progress. Reset or erase it
  from **Profile**. No real applications or charges.
- Email signup/login (connected mode); private PDF/DOCX/text resume parsing with spaCy rules and an
  editable, reviewable profile.
- Role/location/annual salary/work-mode preferences and **seven-dimension explainable matching**
  with a coverage figure, plus a **what-if match lab** that never edits the profile.
- Animated swipe feed (left dismiss, up save, right interested) with equivalent accessible buttons,
  search and filters.
- Per-application consent, missing-field handling, idempotent demo-employer submission, external
  redirects, and a tracker that keeps demo receipts, "Finish on source" and "Self-reported" apart.
- Learning modules (three lessons and a three-question check each). Passing a quiz never changes the
  skills on your profile.
- Persistent notification inbox; Firebase FCM registration/delivery in connected mode.
- Server-enforced daily quotas, Premium feature gates, and a labelled demo subscription.
- Job-specific skill-gap guidance and interview practice prompts (deterministic, not generative).
- Aggregate admin panel, Firebase rules/indexes, and a bounded scheduled matching function.
- A Material 3 interface that adapts from phone to desktop widths, with a bundled font.

## Quick start: offline showcase (no backend needed)

Requires Flutter (stable). From the repository root:

```powershell
Set-Location mobile
flutter pub get
flutter run
```

On the first screen choose **Explore the showcase**. Everything runs on the device with fictional data.

- **APK:** `flutter build apk --debug` creates `mobile\build\app\outputs\flutter-apk\app-debug.apk`.
  Install it with `adb install -r`. It is a debug build signed with the debug key, for demonstration only.
- **Web (classroom demo):** build with
  `flutter build web --release --dart-define=API_BASE_URL=http://127.0.0.1:8000`, then serve it with
  `python -m http.server 8765 --bind 127.0.0.1 --directory build\web` (run from `mobile`) and open
  <http://127.0.0.1:8765>.

## Quick start: connected mode (local backend)

Requires Python 3.12. Run these in PowerShell from the repository root:

```powershell
python -m venv backend\.venv
backend\.venv\Scripts\python.exe -m pip install -r backend\requirements.txt
.\scripts\Start-Demo.ps1
```

If your PowerShell policy does not permit scripts, run the equivalent commands
without changing the machine's execution policy:

```powershell
backend\.venv\Scripts\python.exe scripts\configure_demo.py
backend\.venv\Scripts\python.exe scripts\create_demo.py
Set-Location backend
.\.venv\Scripts\python.exe -m uvicorn app.main:create_app --factory --host 127.0.0.1 --port 8000
```

The setup generates **untracked random secrets** in `backend\.env` and persistent
SQLite data in `backend\data`. Existing files and user data are preserved.

- API health: <http://127.0.0.1:8000/health>
- Interactive API docs: <http://127.0.0.1:8000/docs>
- Admin panel: <http://127.0.0.1:8000/admin> (use the local `ADMIN_SECRET` from `.env`)
- **Demo-only login:** `demo@example.com` / `SkillSync-Demo-2026!`
- Sample resume: `demo\alex-demo-resume.pdf`, generated by `scripts\create_demo.py`

These credentials are intentionally public and work only with the local demo seed.
Do not host demo mode publicly or use it with real candidate data.

## Connect the Android app to the backend

Install Flutter and Android Studio, including the Android SDK, command-line tools,
and an emulator. Open **the `mobile` folder** in Android Studio, not the backend.
Choose **Connect your account** on the first screen once the backend is running.

```powershell
Set-Location mobile
flutter pub get
flutter doctor --android-licenses
flutter doctor
flutter run --dart-define=API_BASE_URL=http://10.0.2.2:8000
```

`10.0.2.2` reaches the host computer from the standard Android emulator.
For a USB-connected phone, enable USB debugging and run:

```powershell
adb reverse tcp:8000 tcp:8000
flutter run --dart-define=API_BASE_URL=http://127.0.0.1:8000
```

Only emulator/loopback cleartext traffic is permitted in Android **debug** builds.
Release builds require HTTPS. Do not solve connectivity by exposing the demo server
to the public internet.

To copy the synthetic PDF to a running emulator:

```powershell
adb push ..\demo\alex-demo-resume.pdf /sdcard/Download/alex-demo-resume.pdf
```

The Android device path above is an Android path, not a Windows filesystem path.

## Demonstration

Follow [docs/DEMO_SCRIPT.md](docs/DEMO_SCRIPT.md): a timed eight-minute walkthrough of the offline
showcase with the exact on-screen text to expect, fallbacks, and viva questions.

To show the connected backend as well, start it as above and sign in with the demo account. Reviewed jobs
stay reviewed across restarts there, so create a new demo account for a fresh walkthrough; do not erase
team data. In the offline showcase, **Profile > Reset showcase** starts again.

## Connected Firebase / Adzuna setup

See [deployment and production boundaries](docs/DEPLOYMENT.md). Cloud services require
your own Firebase project, credentials, billing decisions, and job-source agreement.
No Firebase project, paid services, real payment product, or Adzuna account is created
automatically.

**Current boundaries:** no arbitrary portal auto-apply, no generative resume tailoring,
no OCR, no real checkout, no production institution dashboard, and no promise of revenue.
These are explicit roadmap items, not disguised UI simulations.

## Validation

```powershell
Set-Location backend
.\.venv\Scripts\python.exe -m pytest -q
Set-Location ..\mobile
flutter analyze
flutter test
flutter build apk --debug
```

Backend tests (28) cover the connected demo flow, authentication/ownership, PDF/DOCX/text parsing,
quotas under concurrency, missing information, notifications, salary/work-mode scoring, idempotency,
ambiguous provider outcomes, learning quizzes, bookmarks, and local-only CORS. Flutter tests (60)
cover Python-to-Dart scoring parity (19 golden cases), the offline store, the user journeys, profile
validation, and layouts at 320 px, 360 px with 200 % text, and 1440 px.

If you change a job, a lesson or a scoring rule in Python, regenerate the offline assets and run the
drift test, which fails when the committed assets no longer match the Python source:

```powershell
backend\.venv\Scripts\python.exe scripts\export_showcase.py
Set-Location backend
.\.venv\Scripts\python.exe -m pytest -q tests\test_showcase_features.py
```

To make a clean source ZIP (no secrets, builds or local data) outside the repository:
`backend\.venv\Scripts\python.exe scripts\package_source.py C:\path\outside\SkillSync-source.zip`

## Team contributions

Read [CONTRIBUTING.md](CONTRIBUTING.md) and fill in [docs/CONTRIBUTION_WORKSHEET.md](docs/CONTRIBUTION_WORKSHEET.md).
Each student must actually implement/review their own contribution and push commits using their own
GitHub account and verified email. Never fabricate participation, share accounts, or assign commits
to a teammate. An AI-generated starter does not replace individual course participation requirements.
