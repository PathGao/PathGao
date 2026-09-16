# Personal repository governance implementation plan

Goal: keep a reusable governance baseline in PathGao and align Nifro and kururu.

Approved design: ordinary `governance/` directory; manual adoption; common settings,
short templates and labels; project-specific CI and release workflows. The user
clarified on 2026-09-16 that completed issues close on merge using `Closes` or `Fixes`.
Do not migrate the release/confirmation state machine or its automatic comments.

Execution stays in this task, in isolated checkouts based on each remote main.
Application code and the existing local branches remain outside this change.

- [x] A1: Create settings, labels, ruleset skeletons, templates and adoption guide.
- [x] A2: Align kururu; retire the inactive upstream issue workflow and sponsor config.
- [x] A3: Align Nifro; preserve project forms and checks, simplify PR guidance.
- [x] A4: Validate files and label-sync behavior before applying remote settings.
- [x] A5: Apply labels, merge settings, protection and private reporting; read back.

Validation expectations: PR templates match and permit closing keywords; label
sync preserves labels outside its manifest; required check contexts match CI;
settings and labels read back match their manifests; source templates stay outside
PathGao's active `.github` directory. No app build is needed for these metadata edits.
A real merge is a separate publication check, not simulated by creating public PRs.

The user approved committing, pushing and opening all three PRs. Keep pre-change
remote snapshots in each repository's Git metadata for comparison and rollback.

Independent local review: no findings. CI re-enablement remains a separate user decision. Publish the three approved
changes as PRs without merging; remote manifests match the applied baseline.
