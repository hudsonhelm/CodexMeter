# HudsonHelm download publication staging

Use this folder for the reviewed CodexMeter website content change once the current site and deployment method have been inspected. No production HTML or guessed download URL is staged yet.

The final link must identify the tested Hudson Helm fork version and Windows installer, resolve to that installer or version-specific release page, and provide prerequisites, release notes and checksum access. A portable ZIP may be offered separately but cannot replace the required installer. Follow `../RUNBOOK.md`; record matching downloaded installer hash, clean installation/tray/settings smoke test and rollback in `../../validation/ACCEPTANCE.md`.

Keep production backups and credentials outside committed website content. Preserve the site's current framework, navigation, design and unrelated content.

Latest format preference: one `.msi` installer, otherwise one `.exe`. Label the actual format clearly, explain runtime/offline requirements accurately, and do not substitute a portable ZIP or bare app executable.
