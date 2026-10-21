# CodexMeter project instructions

- Prefer single-line PowerShell commands in copyable code blocks when giving commands to Commodore.
- Read `PROJECT_PLAN.md`, `STATUS.md`, `docs/project/DECISIONS.md`, and the original `CodexMeter-banked-reset-credits-handoff.md` before continuing project work. Read `docs/release/RUNBOOK.md` before packaging or publishing.
- Preserve the handoff as supplied. Its setup assumptions are historical; use verified repository state in `STATUS.md`.
- Preserve the upstream source layout. Keep project planning under `docs/project/`, validation under `docs/validation/`, and release work under `docs/release/`.
- Implement only the available banked reset count through the existing refresh path. Preserve unknown versus zero. Do not redeem resets or add authentication, scraping, telemetry, polling requests, or model calls for this feature.
- Work on `feature/banked-reset-credits`; do not merge into `main` without an explicit instruction. Inspect status and remotes before Git operations. Never discard user work.
- Record actual commands, results, limitations, commit IDs, and artifact hashes. A checklist is not evidence of passing a check.
- Keep credentials, account responses, personal diagnostics, prompt rules, and signing material out of commits and release packages. Put private local evidence under ignored `artifacts/private/`.
- Website work is limited to the CodexMeter download content/link. Inspect the current site and deployment path first; preserve mail, DNS, Exchange, certificates, and unrelated website content.
- Project completion requires all release gates in `PROJECT_PLAN.md`, including Commodore's confidence in sharing and a verified public HudsonHelm download link. Feature completion alone is insufficient.
