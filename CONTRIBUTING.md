# SkillSync collaboration workflow

The shared repository is https://github.com/TechWhizSS/skillsync_mad_project.
Use `main` for reviewed integration. Work on `feature/member-1`,
`feature/member-2`, and `feature/member-3`; do not force-push or merge without
review. The source restoration prerequisite and proposed ownership are recorded
in [docs/TEAM_HANDOFF.md](docs/TEAM_HANDOFF.md).

## Honest authorship

Ownership is a future responsibility, not evidence of who wrote existing code.
Retain existing history and author identities. Do not move already committed
code into new commits to create contribution credit. An import records an
import; it does not establish authorship of all imported files.

Each teammate should configure their own name and verified commit email only
on their own computer, implement or substantially review and improve their
assigned work, run its checks, and commit what they actually did. Record concrete
files, behavior, tests, and any AI assistance in the PR. Use co-author trailers
only for genuine, approved co-authorship. Never share passwords or tokens.

Codex-generated setup changes are attributed to Codex, not to a student. Branch
names and planned assignments do not imply student participation. Resolve any
disputed authorship with the people involved before recording it.

## Before staging

Run `git status --short`, then add only explicit reviewed paths. Check
`git diff --cached --check` and `git diff --cached` before committing. Inspect
untracked and ignored files when importing the complete project. Do not commit
real resumes, candidate data, local databases, `.env` files, service accounts,
OAuth secrets, signing keys, passwords, or payment credentials. The ignore file
is a safeguard, not a substitute for reviewing content; it cannot untrack secrets
already committed. Rotate an exposed secret instead of merely deleting its file.

## Application checks after source restoration

Run from the repository root in Bash; these commands describe the supplied
project documentation and have not been verified against source in this checkout:

```bash
python3.12 -m venv backend/.venv
backend/.venv/bin/python -m pip install -r backend/requirements.txt
(cd backend && .venv/bin/python -m pytest -q)
(cd mobile && flutter pub get && dart format --output=none --set-exit-if-changed lib test)
(cd mobile && flutter analyze && flutter test && flutter build apk --debug)
```

Use repository version pins and lockfiles when the source is available. Add a
focused behavior test for each implementation change. Verify tests actually
execute; report failures, skips and unrun checks separately. A formatter or an
empty test run does not establish that the application works.

If jobs, lessons, scoring or hiring plans change, the supplied documentation
also calls for regenerating showcase assets and checking parity:

```bash
backend/.venv/bin/python scripts/export_showcase.py
(cd backend && .venv/bin/python -m pytest -q tests/test_showcase_features.py)
(cd mobile && flutter test test/hiring_test.dart)
```

The backend owner controls shared Python fixtures and the exporter; coordinate
generated asset updates with the mobile owner. Review all generated changes.

## Pull requests and integration

Use `gh pr create --base main --head feature/member-N` after pushing a meaningful
passing change. Describe the behavior, actual author(s), exact checks and remaining
limitations. Obtain a teammate's review. Prefer a reviewed merge commit (without
squashing or rebasing away individual commits) when retaining individual history
matters. Never treat a branch pointer or an empty PR as a contribution.

After merging, verify each genuine author's commits are reachable from `main`
with `git log origin/main --format='%h %an <%ae> %s'`. Authors must link their
commit email to the correct GitHub account. The GitHub contributors page may take
time to update; it is not evidence that an assigned member actually did the work.
