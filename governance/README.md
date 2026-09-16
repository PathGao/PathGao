# Personal repository governance

Baseline: **2026-09-16, Markpad labels**. Source: [PathGao/governance](https://github.com/PathGao/PathGao/tree/main/governance).
Adoption is explicit and manual. Files here are source material, outside the
profile repository's active `.github` directory. PathGao itself adopts the shared
settings and labels through the API; storing templates here does not apply them.

## Policy

- Delete source branches after merge; use squash; allow opt-in auto-merge.
- Require a PR for the default branch; block deletion and force pushes. No
  external approval is required for a solo maintainer. Repository administrators
  can bypass the branch rules for recovery; this is an explicit exception.
- Require the project's active CI checks when adopting the branch skeleton.
  CI/release implementations stay project-specific. Never require a disabled job.
- Protect `v*` release tags from updates/deletion, with no administrator bypass.
  `v*-test*` tags are exempt. New tags remain allowed.
- Use the short PR template. `Closes`/`Fixes` closes completed issues on merge.
  Partial work uses a normal reference and names the remainder. No release-status
  labels, confirmation timer, automatic issue comments or inactivity closure.
- Use Markpad's label names, colors and descriptions as recorded on 2026-09-16,
  excluding `wontfix` and its JavaScript/Rust language labels. `question` is a
  usage question; `needs info` waits for a reporter; `awaiting decision` waits
  for a maintainer decision; `planned` means accepted. `Final_Check_Request`
  is a manual marker for work that still needs confirmation or is incomplete.
  Keep those issues open with normal references until the work is complete.
  There is no bot or mandatory `Refs` syntax. Project labels extend this set.
- Remove unused default labels `wontfix`, `good first issue`, `help wanted`,
  `invalid` and `duplicate` during adoption. Inspect open and closed issues/PRs
  before deleting; preserve meaningful project labels. A duplicate can be
  closed with a link, and a declined request with an explanation.
- Enable private vulnerability reporting and document the current reporting
  channel. Do not copy another project's signing claims, sponsor identity,
  license or promised response time.

## Files

| File | Responsibility |
| --- | --- |
| `settings.json` | Common merge settings; apply through GitHub's API |
| `labels.json` | Shared label names, colors and descriptions |
| `rulesets/main.json` | Branch protection skeleton; add the project's active CI checks |
| `rulesets/version-tags.json` | Release-tag protection for projects using `v*` tags |
| `templates/.github/` | Short PR, issue and community-document starting points |
| `templates/Tools/sync-labels.sh` | Preview-first JSON label synchronization |

## Adopt in a repository

1. Copy the appropriate templates, `settings.json`, `labels.json` and `rulesets/`
   into the target repository (`.github/` for manifests, `Tools/` for the script).
   Preserve existing project-specific content and labels. Replace `{{REPOSITORY}}`
   with `OWNER/REPO` in copied files. Complete project build/test/security details.
   Select a license separately; the profile repository's contents are not a
   project skeleton. Existing repositories retain their code of conduct.
2. Add `required_status_checks` to the copied main ruleset using actual job names
   from the project's enabled CI. For example, a rule for a job named `Build`:

   ```json
   {
     "type": "required_status_checks",
     "parameters": {
       "strict_required_status_checks_policy": false,
       "do_not_enforce_on_create": false,
       "required_status_checks": [{"context": "Build"}]
     }
   }
   ```

   Check that every required job runs on ordinary PRs and has a successful run.
   If CI is deliberately disabled, omit this rule and record the exception.
3. Inspect current labels and rulesets before applying anything. Run these from
   the target checkout, after setting `target_repo` to its explicit `OWNER/REPO`:

   ```sh
   ./Tools/sync-labels.sh "$target_repo"
   gh api "repos/$target_repo/rulesets"
   ```

   The label script validates the full manifest before writes, previews by
   default, and never deletes undeclared labels. Apply approved label changes:

   ```sh
   ./Tools/sync-labels.sh "$target_repo" --apply
   ```

   The preview only covers declared labels; zero changes does not mean default
   labels have been removed. Check the five retired names above separately.
   If unused, explicitly delete each approved label, for example:

   ```sh
   gh label delete "wontfix" --repo "$target_repo" --yes
   ```

4. Apply repository settings and enable private reporting:

   ```sh
   gh api --method PATCH "repos/$target_repo" --input .github/settings.json
   gh api --method PUT "repos/$target_repo/private-vulnerability-reporting"
   ```

   For each ruleset, read its current detail and preserve unrelated rules. Use
   `POST repos/$target_repo/rulesets --input FILE` only for a new ruleset; use
   `PUT repos/$target_repo/rulesets/ID --input FILE` to update an existing one.
   Do not create a second ruleset to replace an existing ruleset of the same role.
5. Read back settings, labels, reporting and ruleset details. Confirm no label
   changes remain in preview. After publishing the files, verify an ordinary
   PR gets the template and required checks, then verify source-branch deletion
   on a real approved merge. Do not create disposable public issues for tests.

## Updating adopted repositories

Record this baseline version in the target's `.github/GOVERNANCE.md`. Treat that
as provenance, not a promise of automatic synchronization. Compare updated
source files with each target, preserve documented project differences, and
review changes before applying them. Merge settings, labels and rulesets need
API application as well as committed files. No bot, cross-repository token or
scheduled synchronization is required.

## PathGao itself

PathGao adopts `settings.json`, `labels.json` and `rulesets/main.json` directly.
Its main branch requires PRs and blocks deletion/force pushes with the same
administrator recovery bypass. This profile repository has no CI or release
pipeline, so it has no required CI checks or version-tag ruleset.

From the PathGao checkout, preview the authoritative label manifest directly:

```sh
./governance/templates/Tools/sync-labels.sh PathGao/PathGao --labels "$PWD/governance/labels.json"
```

Add `--apply` to apply it. Apply settings with
`gh api --method PATCH repos/PathGao/PathGao --input governance/settings.json`.
The templates remain source material; there is no second label manifest to drift.

Nifro keeps its site submission form, `accessibility`, `site submission`, `swift`
and `upstream` labels, and five CI checks. kururu keeps `accessibility`, its
feature-area reporting fields and project build workflow. The common ten labels
are identical across all three repositories.
