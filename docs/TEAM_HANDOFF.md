# Team handoff and source restoration prerequisite

## Verified state

The selected origin is `https://github.com/TechWhizSS/skillsync_mad_project.git`.
At inspection, remote `main` was
`7934a56ef169a335e8b7defede68ce5b36166e78`, authored by
`Suraj SIngh <officeofsurajsingh@gmail.com>` with subject `Add files via upload`.
Its only tracked file is `README (1).md`. That original file is preserved.

Four attachments were supplied: `README.md`, `CONTRIBUTING.md`, `.gitignore`, and
`firebase.json`. They describe an application but do not supply its source,
dependency manifests, tests, Firebase rules/functions, scripts or assets. The
newer uploaded README describes version 2.1; the committed README describes 2.0.
Neither document's historic test claims are results from this session.

No complete application was found under `/workspace`. Source was not recreated
from prose. The uploaded Firebase configuration is not installed: every rules
and functions path it references is absent. The supplied `.gitignore` was used
as the basis for the new exclusions. Other attachments are retained outside the
checkout, without overwriting the original README.

**Required next input:** the complete source archive, or the repository/branch or
workspace path containing the actual project. Import it through a reviewed PR,
inspect it for secrets, preserve any original Git history, and establish a
passing baseline before calling feature work complete. Do not commit a source
archive instead of its extracted source files.

## Proposed workstreams, pending source inspection

These are responsibilities for future work, not claims of original authorship.
Paths below come from the supplied documentation or are proposed new test paths;
none of the application paths exists in the current baseline. Exact existing
screen, service and API filenames must be inventoried when the source arrives.

| Member / branch | Scope and proposed file ownership | Completion evidence |
| --- | --- | --- |
| Merant (`merantpatel`), `feature/member-1` | Flutter user journeys, resume upload and interview/offer presentation. Own `mobile/lib/` and `mobile/test/`, including the documented `mobile/lib/domain/hiring.dart` and `mobile/test/hiring_test.dart`; own `mobile/pubspec.yaml`, its lockfile and Android client configuration once inspected. | Analyze/build the app; test upload error handling, invitation navigation and offer decisions; keep hiring parity tests passing; verify an offline journey. |
| Suraj Singh (username supplied as `TevhWhizSS`), `feature/member-2` | Backend parsing, matching and interview state transitions. Own `backend/app/`, `backend/tests/`, backend dependency manifests, `backend/app/hiring.py`, `backend/app/seed.py`, `backend/tests/test_showcase_features.py` and `scripts/export_showcase.py`. | Run the complete backend suite and focused parsing/state-transition/idempotency tests; regenerate and verify shared showcase fixtures; exercise the connected API with the mobile owner. |
| Aarush (`aarushnsaraf-bit`), `feature/member-3` | Firebase authorization, scheduled matching, push delivery and reproducible integration. Own `firebase/`, root `firebase.json`, `.gitignore`, `.github/`, deployment docs and integration scripts except `scripts/export_showcase.py`; add focused emulator/scheduler tests only after source inspection. | Prove cross-user denial and authorized paths in emulator tests, scheduled-job bounds and notification deduplication; document repeatable startup; run an integrated smoke check. Real provider checks require the team's own configured services. |

Comparable complexity is an intended allocation; it cannot be confirmed without
the code. Rebalance after inspection. `TevhWhizSS` and the repository owner
`TechWhizSS` differ in spelling; do not assume they are the same account or send
an invitation until that is resolved. Existing commit metadata supports Suraj's
baseline commit only. No supplied evidence attributes application sections to
Merant or Aarush.

## Interfaces and controlled shared ownership

- Preserve existing API routes, request/response payloads, schemas and environment
  variable names once inspected. Backend owns changes to these contracts; mobile
  and infrastructure owners review them before implementation.
- Keep Python/Dart hiring behavior consistent using shared fixtures and existing
  parity tests. Backend owns fixture generation; mobile owns Dart behavior and
  asset-consumption changes. Do not duplicate or hand-edit generated assets.
- Infrastructure owns root configuration, CI, Firebase rules/indexes and deployment
  instructions. Runtime secrets stay in secure environment settings, never in Git.
  Backend and mobile owners retain their respective dependency/lockfile ownership.
- Agree notification payloads, scheduled matching timing and authentication rules
  at the source-baseline review. Do not invent contracts from README summaries.

Recommended review/merge order: review the complete source baseline first, then
agree shared contracts/configuration, review backend changes, mobile changes,
and finally infrastructure integration. Apply any minimal shared prerequisites
before dependent changes. Rerun the full integrated checks before each merge.
No automatic merge is authorized.

## Branch and attribution status

All three branches start at the preserved baseline. `feature/member-1` and
`feature/member-2` initially have no new commits; they are reserved work branches.
`feature/member-3` carries only actual collaboration setup changes, authored by
Codex. It is not an Aarush application contribution. The push and PR results,
including exact final SHAs, are reported separately after execution.

There is no basis for three meaningful application PRs yet. GitHub cannot create
a PR with no changes relative to `main`. Do not use empty commits to work around
this. Once source and real changes are available, keep the branches separate and
open one reviewed PR per workstream.

## Commands on each teammate's computer

Use a fresh destination directory; each person authenticates with their own
GitHub account and accepts any required repository invitation. Do not copy another
person's token or configure another person's author identity. The repository
administrator must confirm the correct Suraj account before granting access.

Merant runs:

```bash
git clone https://github.com/TechWhizSS/skillsync_mad_project.git skillsync-merant
cd skillsync-merant
git config user.name "Merant"
git config user.email "merant.patel@gmail.com"
git switch --track origin/feature/member-1
```

Suraj runs:

```bash
git clone https://github.com/TechWhizSS/skillsync_mad_project.git skillsync-suraj
cd skillsync-suraj
git config user.name "Suraj Singh"
git config user.email "officeofsurajsingh@gmail.com"
git switch --track origin/feature/member-2
```

Aarush runs:

```bash
git clone https://github.com/TechWhizSS/skillsync_mad_project.git skillsync-aarush
cd skillsync-aarush
git config user.name "Aarush Saraf"
git config user.email "aarushnsaraf@gmail.com"
git switch --track origin/feature/member-3
```

After the complete baseline is reviewed and merged, each teammate runs:

```bash
git status --short
git fetch origin
git merge origin/main
# Implement your actual assigned changes and run the checks in CONTRIBUTING.md.
git diff --check
git status --short
```

Stage only the paths personally changed using `git add -- path/to/actual/file`
(replace the example with real paths), inspect `git diff --cached`, then:

```bash
git diff --cached --check
git commit -m "Describe the behavior actually implemented and tested"
git push -u origin HEAD
gh pr create --base main
```

If conflicts occur, inspect and resolve them with the relevant owner, rerun
checks and finish the merge. Do not force-push. If a PR already exists for the
branch, update that PR instead of creating a duplicate.

## Running the complete application after source restoration

These steps are taken from supplied documentation, **not executed successfully
in this checkout**. First inspect the restored version's requirements and pins.
See CONTRIBUTING.md for install/test commands. For a local connected demo:

```bash
backend/.venv/bin/python scripts/configure_demo.py
backend/.venv/bin/python scripts/create_demo.py
(cd backend && .venv/bin/python -m uvicorn app.main:create_app --factory --host 127.0.0.1 --port 8000)
```

In another terminal, check `curl --fail http://127.0.0.1:8000/health`. Follow with
a representative existing API smoke test; an open port is insufficient. For the
offline showcase, run `cd mobile`, `flutter pub get`, then `flutter run` on a
supported device and choose **Explore the showcase**. For Android connected
mode, the documented emulator API base is `http://10.0.2.2:8000`. Do not expose
the local demo server publicly. Firebase, Google sign-in, FCM and Adzuna require
separate verified configuration; do not invent credentials.

Backend tests, Flutter analysis/tests, parity tests, APK/web builds and integrated
application checks are **unrun: required source is absent**. Python 3.12 is
available in this environment; Flutter and Dart were not found on PATH.

## Verify genuine contributions after reviewed merges

```bash
git fetch origin
git log origin/main --format='%h %an <%ae> %s'
git shortlog -sne origin/main
```

Inspect the actual patches, not just names/counts. Check that each member's
genuine commits are ancestors of `origin/main`, using
`git merge-base --is-ancestor COMMIT_SHA origin/main` with each real commit ID.
Each person must verify their commit email in their own GitHub settings. Check
https://github.com/TechWhizSS/skillsync_mad_project/graphs/contributors after
GitHub processes the default branch. Do not promise all three will appear until
their genuine commits have been merged and GitHub links their verified emails.
