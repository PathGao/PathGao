# Adoption record — 2026-09-16

## Prepared files

- PathGao: manual governance baseline, templates, label tool and adoption guide.
- kururu: short PR template, shared labels/settings, main/tag rulesets, private
  reporting policy, project forms and governance notes. Retire the inactive
  upstream issue-state workflow and remove its sponsor account from active config.
- Nifro: the same PR/label/settings baseline; preserve site forms, project labels,
  five CI checks and existing protection. Replace the YAML label manifest and
  hand-written YAML parser with JSON.

The user selected close-on-merge using `Closes`/`Fixes`. No release-state labels,
confirmation timer, automatic comments or inactivity-close policy are adopted.
The legacy Nifro label `Final_Check_Request` remains on GitHub for history.

## Remote settings

Both app repositories use automatic source-branch deletion, squash-only merging,
opt-in auto-merge and private vulnerability reporting. kururu gains protected
main and version tags; Nifro retains its existing rules. The inherited kururu
issue workflow is disabled on GitHub pending publication of its removal.

kururu's CI was already manually disabled. Its main rule requires PRs and blocks
deletion/force pushes, with an administrator recovery bypass, but does not require
CI. Enabling CI needs a separate decision and a successful current run before
adding the two check contexts. Nifro's five required checks passed on d7c238d.

## Verification

- JSON/YAML parse, issue-form label references, template equality, resolved target
  repository links, shell syntax and Git whitespace checks pass.
- Seven fake-GitHub label cases pass: read-only preview, no-op, create/update with
  undeclared-label preservation, case rename, invalid manifest, duplicate names,
  and stopping on an API write failure.
- Live label previews report zero remaining changes for both app repositories.
- Remote settings, private reporting and ruleset details are read back against
  manifests. Pre/post snapshots stay in local Git metadata for rollback.

No application source was changed or app build run. Real PR-template rendering,
check gating and post-merge branch deletion remain publication-time checks.

## Publication

Publish the baseline and the two project adoptions as separate pull requests
from `chore/governance-baseline`. Merge PathGao first so the source links resolve,
then merge the two app changes. Existing development branches remain separate.
Real template rendering, required-check gating and source-branch deletion are
verified as these changes land; API readback alone does not test a merge.
