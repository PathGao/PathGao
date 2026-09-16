# Adoption record — 2026-09-16

## Scope

PathGao itself, Nifro and kururu adopt the shared merge settings and ten Markpad
labels. The set copies Markpad's current names, colors and descriptions while
excluding `wontfix`, `javascript` and `rust`. Project extensions remain in each
app's manifest: `accessibility` in kururu; `accessibility`, `site submission`,
`swift` and `upstream` in Nifro.

The five unused defaults (`wontfix`, `good first issue`, `help wanted`, `invalid`,
`duplicate`) are explicitly retired. All open and closed issues/PRs were checked;
none used these labels. The additive sync script preserves other undeclared
labels, so explicit default cleanup is a separate adoption step.

`Final_Check_Request` is part of the shared set for manual use. Completed issues
can close on merge with `Closes`/`Fixes`; incomplete or unconfirmed work stays
open with normal references. No release/confirmation bot or mandatory `Refs`
syntax is adopted.

## Repository settings

All three repositories use automatic source-branch deletion, squash-only merging,
opt-in auto-merge and private vulnerability reporting. Main requires PRs and
blocks deletion/force pushes, with an administrator recovery bypass.

PathGao has no CI or release pipeline and therefore no required checks or release
tag ruleset. Its source manifests are applied directly, without duplicate active
copies. Nifro retains its five required checks and version-tag protection.
kururu retains version-tag protection; its CI remains manually disabled, so main
has no required checks. CI re-enablement is a separate decision.

## Verification

JSON/YAML, template consistency, form-label references, script syntax and Git
whitespace are checked before publication. The label script supports an explicit
manifest path for PathGao's own source. Remote labels are compared against each
complete manifest; source settings and main protection are read back from GitHub.
No application code or build/release workflow changes are needed.

## Publication

The first adoption PRs (PathGao #1, kururu #8 and Nifro #134) were merged.
Corrections start from their current main branches, on `chore/governance-labels`.
Source changes land first; app manifests then record the adopted source revision.
New PRs remain for maintainer review; this work does not merge them automatically.
