# AGENTS.md — layer-check-retention-fixture

Standalone candy repo for the `check-retention-fixture` layer — the fixture half
of the `check-retention` retention-discriminator bed. The candy lives in
`charly.yml` at the repo root: the `write:` marker step and the `check:`
assertions over `/etc/charly-retention-fixture`. The repo declares **no `skill:`
entity**; the owning guidance is the family skill `/charly-check:check` (the gap
is tracked in `opencharly/opencharly#291`).

Canonical files:

- `charly.yml` — the `check-retention-fixture:` candy entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-check:check` — the owning (family) skill. The check bed and `plan:`
  authoring reference, incl. `check:` step verbs and `disposable: true` beds.
  Load before editing or troubleshooting this layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package/repo
  sections, and service declarations). Load before editing any entity field or
  plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence; they must stay
  valid on every distro arm they run on. Scope a distro-specific check in the
  command itself — the check runner does not honour runner-level
  `exclude-distro` fields.

## Modify this repo

- Keep the marker path `/etc/charly-retention-fixture` dedicated — never reuse
  `check-group`'s or `check-local`'s marker, so concurrently fanned-out beds do
  not collide.
- New behaviour claims belong in the `plan:` as an observable `check:` step.
- This repo has no `skill:` entity, so there is no projected corpus to mirror.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
