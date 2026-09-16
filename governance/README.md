# Personal repository governance

Baseline: **2026-09-16**. Source: [PathGao/governance](https://github.com/PathGao/PathGao/tree/main/governance).
Adoption is explicit and manual. Files here are source material, outside the
profile repository's active `.github` directory. They do not configure GitHub by themselves.

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
- Keep shared label meanings consistent. `question` is a usage question;
  `needs info` waits for reporter details; `awaiting decision` waits for a
  maintainer decision; `planned` means accepted. Other labels classify type or
  resolution. Project labels may extend this set.
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

The first adoption is Nifro and kururu. Nifro keeps its site submission form,
site labels and five CI checks. kururu keeps its feature-area reporting fields
and project build workflow. The old `Final_Check_Request` label in Nifro is left
on GitHub for historical issues but is not part of the new managed label set.
