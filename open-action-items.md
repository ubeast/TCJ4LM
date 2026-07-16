# TC4JLM GitLab — Open Action Items

Things discussed and decided in principle, but not yet confirmed as done. Check these
off as they're completed; update `where-does-this-go.md` and
`tc4jlm-gitlab-structure.md` if resolving one of these changes the structure.

---

## 1. Records retention — needs an actual answer, not an assumption

- [ ] Confirm with your records management officer / ISSM whether `ARCHIVE/` content
      falls under an existing NARA/agency records schedule, or whether one needs to be
      requested. Unscheduled records default to "must be kept" under federal rules —
      don't assume a practical cleanup cadence is sufficient until this is confirmed.
- [ ] Ask the same question for `RFIS/` — responses to information requests may carry
      their own retention obligation separate from `ARCHIVE/`'s.

## 2. Restrict group/project creation under `TC4JLM`

- [ ] Confirm whether subgroup/project creation directly under `TC4JLM` is actually
      restricted to admins yet. This was recommended early as the fix for future
      "ORCA-style" sprawl (things created with nowhere else to go) but was never
      confirmed as implemented.

## 3. Rename `Tools` → `j4lm-pylib`

- [ ] Rename the group/project path (Settings > General > Advanced > Change
      [group/project] URL).
- [ ] Update any existing `git clone`/remote URLs pointing at the old path.
- [ ] Update any CI `include:` referencing the old path.
- [ ] Update any pip install lines / `pyproject.toml` references pointing at the old
      Package Registry path.
- [ ] Update documentation links referencing the old URL.
- [ ] Restructure the single existing script into a proper `src/` layout package
      (`pyproject.toml`, tests) rather than a flat script.

## 4. ORCA

- [x] Ownership/status review complete: it's one developer's Git learning project.
- [ ] Move it into her `PLAYGROUND/<username>` namespace.
- [ ] Quick pass to confirm the repo contents reflect real, intentional work rather
      than leftover practice commits, since it may pick up viewers even while staying
      in PLAYGROUND.
- [ ] Revisit if a second person actually starts depending on it (not just being
      encouraged to try it) — that's the trigger to move it to `LABS/`.

## 5. Naming cleanup on existing groups

- [ ] `Archive` → `ARCHIVE` (casing)
- [ ] `personal playground` → `PLAYGROUND` (casing + spacing)
- [ ] `guidance`, `Tools`/`j4lm-pylib`, and other current group names should be
      double-checked against the new full-uppercase-for-groups convention — anything
      that's a group (not a project) needs to match.
- [ ] Confirm `guidance` and `Tools`/`j4lm-pylib` end up nested under a `COMMON/`
      parent group, per the structure documents — this was an assumption carried
      through both documents and never explicitly confirmed.

## 6. Descriptions

- [ ] Apply the finalized Purpose / What goes here descriptions (in
      `where-does-this-go.md`) to each group and project's Description field in
      GitLab (Settings > General).

## 7. Training

- [ ] Decide final project name for the curriculum (`COMMON/gitlab-training` used as
      the placeholder throughout — confirm or rename).
- [ ] Build out first cohort's live workspace once a class date is set:
      `LABS/gitlab-training-<year>-<cohort-id>`.
- [ ] Revisit whether `COMMON/gitlab-training` should move under a `COMMON/training/`
      parent if a second or third class becomes real (per the "don't nest ahead of
      need" guidance).

---

*Delete or check off items as they're resolved. If this file empties out, that's a good
sign — fold anything still worth remembering into the other two documents and archive
this one.*
