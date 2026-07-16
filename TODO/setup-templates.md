# Setting Up COMMON/TEMPLATES and Custom Project Templates

This walks through creating the `COMMON/TEMPLATES` subgroup and configuring GitLab's
custom project template feature from scratch. Nothing exists yet, so there's no
project transfer involved — everything is created directly in its final location.

---

## Step 1: Create the `TEMPLATES` subgroup

1. Go to `COMMON` → **New subgroup**
2. Name it `TEMPLATES` (uppercase, per the group naming convention)

## Step 2: Create the template project directly inside `TEMPLATES`

Create the new project *inside* `COMMON/TEMPLATES` from the start — there's nothing
to move.

- Name: `j4lm-project-template`
- Populate it with the starter `src/` layout, `pyproject.toml`, CI config, and README

## Step 3: Set the project's visibility and features

In `j4lm-project-template`'s **Settings > General**:

- **Visibility**: Public or Internal (Private templates only work for project members,
  which defeats the point of a shared template)
- **Project features**: set to Everyone With Access, **except** GitLab Pages and
  Security & Compliance

## Step 4: Point `COMMON` at `TEMPLATES` as its template source

1. Go to `COMMON` → **Settings > General**
2. Expand **Custom project templates** — this section only appears on **Premium or
   Ultimate** tier. If you don't see it, that confirms your instance isn't licensed
   at that tier (see Fallback below).
3. Select `TEMPLATES` as the subgroup to source templates from
4. Save

## Step 5: Verify it works

1. Create a new test project anywhere under `COMMON`'s scope
2. Choose **Create from template → Group** tab
3. Confirm `j4lm-project-template` appears as a selectable option and correctly
   populates the new project with its `src/` layout, `pyproject.toml`, CI config,
   and README

---

## Fallback: if your instance isn't Premium/Ultimate

If the **Custom project templates** section doesn't appear in Step 4, this native
GitLab feature isn't available. Use `j4lm-project-template` as a manual copy-from
starting point instead:

1. Clone `j4lm-project-template`
2. Remove its `.git` history (`rm -rf .git && git init`)
3. Use the resulting files as the starting point for the new project

---

## Why `TEMPLATES` is a separate subgroup rather than a project directly in `COMMON`

GitLab's custom project template feature designates an entire subgroup as the
template source — every project directly inside that subgroup becomes selectable
as a template. If `j4lm-project-template` sat as a peer next to `guidance`,
`j4lm-pylib`, etc. directly in `COMMON`, all of those would also show up as
"templates" when someone creates a new project — which isn't what you want.
Keeping `TEMPLATES` as its own dedicated subgroup avoids that.
