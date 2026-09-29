# AGENTS.md — layer-steam

Standalone candy repo for the `steam` layer — the Steam gaming client plus the
gamescope nested compositor on a Sway/XWayland desktop. The candy lives in
`charly.yml` at the repo root: the `require:` on `pod-sway`, the `env:`,
`security:`, and `volume:` blocks, the Fedora package section, the `check:`
probes, and the embedded `skill:` entity projected into the marketplace corpus as
`/charly-selkies:steam`.

Canonical files:

- `charly.yml` — the `steam:` candy entity and the `steam-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:steam` — the owning skill. The packages, the XWayland drop-in
  config, the gamescope launch options, and the first-login flow. Load before
  editing or troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, `distro:` sections, `env:`, `security:`,
  `volume:`). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- `charly box validate` at the repo root checks the manifest parses and
  validates.
- The candy's `plan:` `check:` steps are the functional evidence: the Steam
  launcher and gamescope binaries, both RPM packages, and the
  `STEAM_RUNTIME_PREFER_HOST_LIBRARIES=0` env pin.
- `shm_size: 1g` and the `steam-data` volume are part of the runtime contract;
  keep them aligned with the skill.

## Modify this repo

- Edit the `steam:` candy entity AND the `steam-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a package or
  env change not mirrored in the skill leaves the corpus stale.
- Steam is X11: the Sway drop-in enabling XWayland is load-bearing; do not
  disable it.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
