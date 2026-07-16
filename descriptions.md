

A personal experiment, nobody else depends on it
`PLAYGROUND/<your-username>` |

Reusable docs, code, business rules, CI config, or a project scaffold used by 2+ products | `COMMON/` (see which specific repo below) |

Team-level, multiple contributors, not yet owned like a product | `LABS/<name>` |

An owned product with a real owner and users | `PRODUCTS/<name>` |

Something other products are generated off of / depend on (e.g. SDDB) | `PRODUCTS/<name>`, flagged as a **foundation product** |

A one-off response to a specific data request (RFI) | `RFIS/rfi-<year>-<sequence>-<short-slug>` |

Internal training/enablement material (e.g. the Git/GitLab class) | `COMMON/<training-project-name>` — flat, no subgroup, unless multiple classes accumulate |

A live workspace for one training cohort's exercises | `LABS/<training-project-name>-<year>-<cohort-id>` |

Pre-migration backup (Advana requirement) | `ARCHIVE/` |

Genuinely doesn't fit any row above | Don't create it yourself — see Exceptions in `where-does-this-go.md` |







# TC4JLM GitLab Structure — Complete Listing

This is the full current listing of every group and project under `TC4JLM`, with a
detailed description of each. For help deciding where *new* work should go, see
`where-does-this-go.md` — this document is a reference, not a decision guide.

**Legend:** **Group** = organizational container only, holds other groups/projects.
**Project** = an actual Git repository (code, docs, commit history). Groups are named
in full uppercase (`COMMON`, `PRODUCTS`); projects and usernames stay lowercase-kebab-case
(`j4lm-pylib`, `jsmith`) — the casing itself signals which is which.

---

## Quick and dirty: where does this go?

| If your work is... | Put it in |
|---|---|
| A personal experiment, nobody else depends on it | `PLAYGROUND/<your-username>` |
| Reusable docs, code, business rules, CI config, or a project scaffold used by 2+ products | `COMMON/` (see which specific repo below) |
| Team-level, multiple contributors, not yet owned like a product | `LABS/<name>` |
| An owned product with a real owner and users | `PRODUCTS/<name>` |
| Something other products are generated off of / depend on (e.g. SDDB) | `PRODUCTS/<name>`, flagged as a **foundation product** |
| A one-off response to a specific data request (RFI) | `RFIS/rfi-<year>-<sequence>-<short-slug>` |
| Internal training/enablement material (e.g. the Git/GitLab class) | `COMMON/<training-project-name>` — flat, no subgroup, unless multiple classes accumulate |
| A live workspace for one training cohort's exercises | `LABS/<training-project-name>-<year>-<cohort-id>` |
| Pre-migration backup (Advana requirement) | `ARCHIVE/` |
| Genuinely doesn't fit any row above | Don't create it yourself — see Exceptions in `where-does-this-go.md` |

---

## `ARCHIVE` (Group)

**Purpose:** Advana-required backup of pre-migration code. Read-only — not for active development.
**What goes here:** One or more backup projects containing pre-migration snapshots.

### Projects under `ARCHIVE`

*(one or more backup projects — no active naming convention yet, since nothing new is created here)*
- **Purpose:** Preserve a snapshot of code as it existed before the GitLab reorganization.
- **What goes here:** Whatever existed at time of backup. No new commits expected.

---

## `COMMON` (Group)

**Purpose:** Shared resources used across all products — docs, the shared Python library, shared business rules, CI templates, the project scaffold, and internal training material.
**What goes here:** The six projects listed below. Nothing is developed directly in `COMMON` itself — only inside its projects.

### `COMMON/guidance` (Project)

- **Purpose:** Policy, standards, and onboarding docs for TC4JLM.
- **What goes here:** Markdown docs organized into folders (`policies/`, `standards/`, `onboarding/`), with a root index README. Includes `where-does-this-go.md` and this document.

### `COMMON/j4lm-pylib` (Project)

- **Purpose:** Shared, versioned Python utility library.
- **What goes here:** Reusable functions/modules used across products. `src/` layout, published to the GitLab Package Registry. Consumed via a pinned version — never vendored or copy-pasted into a product repo.

### `COMMON/j4lm-business-rules` (Project)

- **Purpose:** Business rules shared by 2+ products.
- **What goes here:** Versioned rule modules, SME-reviewed, semver-tagged. Changes require sign-off — see CODEOWNERS. Rules that only apply to one product stay in that product's own `business_rules/` folder instead.

### `COMMON/j4lm-ci-templates` (Project)

- **Purpose:** Shared CI/CD pipeline definitions.
- **What goes here:** Reusable `.gitlab-ci.yml` includes covering lint, type-check, test, build, and deploy stages. Product repos include these rather than duplicating pipeline stages locally.

### `COMMON/j4lm-project-template` (Project)

- **Purpose:** Starter scaffold for new product repos.
- **What goes here:** Starter `src/` layout, `pyproject.toml`, CI config, and README template. Used when creating any new project under `PRODUCTS/`.

### `COMMON/gitlab-training` (Project)

- **Purpose:** Git/GitLab curriculum for internal training, taught in the Advana Databricks environment.
- **What goes here:** Course material and exercise scaffolding. No `j4lm-` prefix (never referenced outside its GitLab path). Stays flat under `COMMON/` rather than nested under a `training/` subgroup unless a second/third class makes a shared parent worthwhile. The *curriculum* lives here; the *live per-cohort exercise workspace* lives in `LABS/` (see below).

---

## `PRODUCTS` (Group)

**Purpose:** Owned, production-bound work with real users and delivery targets.
**What goes here:** One project per product, listed below.

### `PRODUCTS/sddb` (Project — Foundation product)

- **Purpose:** Core product that other products are generated off of. Maintained by its own dedicated team.
- **What goes here:** Core codebase, `bteq/` (legacy Teradata load scripts, flagged as deprecating), `business_rules/` (sddb-specific rules), release-tagged (semver). Downstream consumers pin to release tags, never to `main`. Does **not** include validation or downstream reporting — those are separate repos maintained by different teams (see below).

### `PRODUCTS/sddb-validation` (Project)

- **Purpose:** Validates each monthly sddb data pull and produces the go/no-go signal analysts check before using that month's data. Maintained by a separate team from sddb's core codebase.
- **What goes here:** Schema checks (row counts, null rates, expected columns/types), business-rule checks (thresholds/ranges tied to sddb's data), and a report module producing the pass/fail signal. Pinned to the sddb release tag it validates. Runs on a monthly schedule.

### `PRODUCTS/sddb-sankey-report` (Project)

- **Purpose:** Quarterly Sankey visualization built from validated sddb output, for analyst use. Maintained by a separate team from sddb's core codebase.
- **What goes here:** Extract/transform/render modules pulling from sddb's validated release. Pinned to a specific sddb release tag. README notes which sddb version and validation pass it depends on. Runs on a quarterly schedule.

### `PRODUCTS/<name>` (Project — template for future products)

- **Purpose:** Owned, production-bound product with real users.
- **What goes here:** Standard `src/`, `tests/`, `pyproject.toml`, `business_rules/` (if any product-specific rules exist), CI config. Built from `COMMON/j4lm-project-template`. See `COMMON/guidance` for repo standards.

---

## `LABS` (Group)

**Purpose:** Team-level, pre-product work — multiple contributors, not yet owned like a product.
**What goes here:** One project per initiative.

### `LABS/<name>` (Project — template)

- **Purpose:** Incubating work with an uncertain future — not yet known whether it becomes a real product.
- **What goes here:** Whatever the initiative needs. Graduates to `PRODUCTS/` once it has an owner, real users, or a delivery target.

### `LABS/gitlab-training-<year>-<cohort-id>` (Project — template)

- **Purpose:** Live exercise workspace for one training cohort (e.g. `gitlab-training-2026-q3`).
- **What goes here:** Whatever that cohort's exercises produce. Created fresh per cohort rather than reused/wiped between sessions — cleaner access control and no reset process to maintain. Archive (don't delete) once the session wraps. The curriculum itself lives in `COMMON/gitlab-training`, not here.

---

## `PLAYGROUND` (Group)

**Purpose:** Individual, unsupported experimentation.
**What goes here:** One subgroup per person (`PLAYGROUND/<username>`).

### `PLAYGROUND/<username>` (Group — one per team member)

- **Purpose:** One person's namespace for their own experimentation.
- **What goes here:** That person's own project(s) — see below.

#### `PLAYGROUND/<username>/<repo>` (Project — template)

- **Purpose:** Individual, unsupported experimentation.
- **What goes here:** One person's scratch work. Not maintained. No one else should build a dependency on it.

---

## `RFIS` (Group)

**Purpose:** One-off responses to specific data requests (RFIs) — not ongoing products.
**What goes here:** One project per request.

### `RFIS/rfi-<year>-<sequence>-<short-slug>` (Project — template)

- **Purpose:** One-off response to a specific data request.
- **What goes here:** The query/script that answered the request, the delivered output (if kept in-repo), and a README recording who asked, what was asked, what was delivered, and the source data version used (e.g. sddb release tag). Archive (don't delete) once roughly a year old with no activity, pending records-management confirmation on formal retention requirements. Graduates to `LABS/` or `PRODUCTS/` if the same request keeps recurring.

---

*This listing reflects the structure as currently designed. Update it whenever a new
group or project is added so it doesn't go stale — an out-of-date reference document
causes the same confusion it's meant to prevent.*


# Where Does This Go? — TC4JLM GitLab Intake Guide

**Purpose:** Before creating a new GitLab group, subgroup, or repo under `TC4JLM`, use this
guide to route your work to the right place. If your situation isn't covered here, don't
create a new top-level location on your own — see **Exceptions** at the bottom.

---

## Quick decision path

1. **Is this just you, exploring, with no one else depending on it?**
   → `PLAYGROUND/<your-username>`

2. **Is it reusable code, docs, business rules, or templates meant for every product?**
   → `COMMON/` (see table below for which repo specifically)

3. **Is it a couple of people building something real, but it's not owned like a product
   yet (no formal delivery target, no SLA)?**
   → `LABS/<name>`

4. **Is it a one-off response to a specific data request (RFI), not an ongoing product?**
   → `RFIS/<rfi-name>`

5. **Does it have an owner, real users, and/or a delivery target?**
   → `PRODUCTS/<name>`

If you land on step 5 and the thing you're building generates or depends on other
products (like `sddb`), flag it as a **foundation product** — see the SDDB note below.
If an RFI keeps getting re-requested and starts looking like an ongoing need, that's the
signal to graduate it to `LABS/` or `PRODUCTS/` rather than leaving it in `RFIS/`.

---

## Location table — organized by group

GitLab **groups** are organizational containers only — they hold other groups or
projects, never code directly. **Projects** are where an actual Git repository lives
(code, docs-as-files, commit history). Each section below is one group under `TC4JLM`,
with everything that lives directly inside it.

### `ARCHIVE` (Group)

| Type | Name | Purpose | What goes here |
|---|---|---|---|
| Project | *(one or more backup projects)* | Advana-required backup of pre-migration code | Snapshots only. Read-only. Nothing new is developed here. |

### `COMMON` (Group)

| Type | Name | Purpose | What goes here |
|---|---|---|---|
| Project | `guidance` | Policy, standards, onboarding docs | Markdown docs organized into folders (`policies/`, `standards/`, `onboarding/`), with a root index README |
| Project | `j4lm-pylib` | Shared, versioned Python utility library | Reusable functions/modules used across products. `src/` layout, published to the GitLab Package Registry, consumed via pinned version |
| Project | `j4lm-business-rules` | Business rules shared by 2+ products | Versioned rule modules, SME-reviewed, semver-tagged. **Not** for rules that only apply to one product |
| Project | `j4lm-ci-templates` | Shared CI/CD pipeline definitions | Reusable `.gitlab-ci.yml` includes (lint, type-check, test, build, deploy) |
| Project | `j4lm-project-template` | Scaffold for new repos | Starter `src/` layout, `pyproject.toml`, CI config, README template |
| Project | `gitlab-training` | Git/GitLab curriculum and exercise setup | Course material and exercise scaffolding for the Git/GitLab class. No `j4lm-` prefix — never referenced by bare name outside its GitLab path |

### `PRODUCTS` (Group)

| Type | Name | Purpose | What goes here |
|---|---|---|---|
| Project (foundation) | `sddb` | Other products are generated off it | Core codebase, `bteq/` (legacy Teradata load scripts, flagged as deprecating), `business_rules/` (sddb-specific), release-tagged (semver). Owned by its own dedicated team — does not include validation or downstream reporting. |
| Project | `sddb-validation` | Monthly go/no-go check on sddb's data pull | Schema checks, business-rule checks tied to sddb's data, and a report producing the pass/fail signal analysts check. Pinned to the sddb release it validates. Owned by a separate team from sddb's core codebase. |
| Project | `sddb-sankey-report` | Quarterly Sankey visualization for analysts | Extract/transform/render pulling from sddb's *validated* output. Pinned to a specific sddb release tag. Owned by a separate team from sddb's core codebase. |
| Project | `<name>` | Owned, production-bound work with real users | Standard `src/`, `tests/`, `pyproject.toml`, `business_rules/` (if any product-specific rules exist), CI config |

### `LABS` (Group)

| Type | Name | Purpose | What goes here |
|---|---|---|---|
| Project | `<name>` | Team-level, multi-contributor, pre-product work | Incubating ideas that outgrew PLAYGROUND but aren't ready to carry product-level ownership expectations |
| Project | `gitlab-training-<year>-<cohort-id>` | Live exercise workspace for one training cohort | e.g. `gitlab-training-2026-q3`. Created fresh per cohort rather than reused/wiped. Archive (don't delete) once the session wraps. Curriculum itself lives in `COMMON/gitlab-training`, not here. |

### `PLAYGROUND` (Group)

| Type | Name | Purpose | What goes here |
|---|---|---|---|
| Group | `<username>` | One person's namespace | Holds that person's own project(s) — see below |

**Under each `PLAYGROUND/<username>` (Group):**

| Type | Name | Purpose | What goes here |
|---|---|---|---|
| Project | `<repo>` | Individual, unsupported experimentation | One person's scratch work. No one else should build a dependency on it. |

### `RFIS` (Group)

| Type | Name | Purpose | What goes here |
|---|---|---|---|
| Project | `rfi-<year>-<sequence>-<short-slug>` | One-off response to a specific data request | The query/script that answered the request, the delivered output (if kept in-repo), and a README recording who asked, what was asked, what was delivered, and the source data version (e.g. sddb release tag) used |

---

## How to decide: shared vs. product-specific (code, rules, or scripts)

This question comes up constantly — for utility functions, business rules, and scripts
alike. Use this rule of thumb:

- **Two or more products currently apply the same logic** → belongs in the relevant
  `COMMON/` repo (`j4lm-pylib` for utility code, `j4lm-business-rules` for business logic).
- **It only makes sense in the context of one product's data or process** → stays local,
  inside that product's own repo.

Do **not** promote something to `COMMON/` preemptively "in case something needs it
later." Rules and utilities get promoted only once actual reuse exists — speculative
sharing is how shared repos become untrustworthy junk drawers.

---

## Naming conventions

- **Subgroups are named by function/lifecycle**, not content type:
  `PRODUCTS`, `COMMON`, `LABS`, `PLAYGROUND`, `ARCHIVE`.
- **Repos are named for what they specifically are**, not a vague category.
  Prefer `j4lm-pylib` over `Tools`, `j4lm-business-rules` over `Rules`.
  A generic name invites unrelated things to pile in — that's how `ORCA` ended up loose
  in `PLAYGROUND` with nowhere better to go.
- **Prefix `j4lm-` only on things consumed by name outside the group hierarchy** —
  packages installed via pip, artifacts referenced from a package registry, anything
  that could show up in a flat namespace without its group path attached. Test:
  *"Will this ever be referenced by name alone, without its group path attached?"*
  If yes, prefix it (`j4lm-pylib`, `j4lm-business-rules`, `j4lm-ci-templates`). If no,
  skip the prefix — the group path already disambiguates it (`COMMON/guidance`, not
  `COMMON/j4lm-guidance`).
- **Groups are fully uppercase; projects and usernames stay lowercase-kebab-case.**
  This makes the group/project distinction visible in the path itself without needing
  to check the Type column: `COMMON/j4lm-pylib`, `PRODUCTS/sddb`,
  `PLAYGROUND/jsmith/notes-app`. Per-user subgroups under `PLAYGROUND` keep the
  person's normal username casing (e.g. `jsmith`, not `JSMITH`) rather than being
  uppercased themselves — they're tied to an actual identity, not a functional category.
  No spaces in any group or project name.
- **RFI projects follow `rfi-<year>-<sequence>-<short-slug>`**
  (e.g. `rfi-2026-014-monthly-tonnage-summary`). Date and sequence number keep them
  sortable and make duplicate/related requests easy to spot at a glance.

---

## Foundation products (e.g. SDDB)

A foundation product is anything other products are generated from or built on top of.
Treat it differently from a standard product:

- **Real release tags (semver)**, not tracking the default branch. Downstream products
  generate off a specific tagged version, never off whatever's currently on `main`.
- **Downstream products declare which version they were generated from** (README or a
  manifest file), so a break can be traced immediately to a version mismatch instead of
  a guessing game.
- **Legacy components inside a foundation product** (e.g. SDDB's current BTEQ scripts)
  stay scoped inside that product's repo, clearly labeled as legacy/deprecating, rather
  than moved to `COMMON/` — they're a live production dependency, not a reusable asset,
  and labeling them as shared would invite copying that then becomes hard to deprecate.

---

## RFIs

RFIs (ad hoc data requests) are a distinct category — not personal experimentation, not
team incubation, not an owned product. Each RFI is a one-off response to a specific
request, with a short expected shelf life.

- **Retention may be a formal records question, not just a housekeeping one** — the
  same caution that applies to `ARCHIVE/` applies here: confirm with your records
  management officer whether responses to information requests carry a retention
  obligation before assuming a practical cleanup cadence is sufficient.
- **Practical default, pending that confirmation:** archive (don't delete) an RFI project
  once it's roughly a year old with no further activity.
- **Graduation:** if the same request keeps coming back, that's the signal to promote it
  to `LABS/` (if its ongoing shape is still uncertain) or directly to `PRODUCTS/` (if
  it's clearly becoming a recurring, owned deliverable) — the same graduation logic as
  everything else, just triggered by "this keeps getting re-asked" instead of "this
  picked up a second contributor."

---

## PLAYGROUND vs. LABS: the actual test

This distinction comes up constantly and is easy to get wrong in both directions, so
use this test rather than a gut call:

1. **Does it have a real owner** (not just a creator)?
2. **Does anyone besides the creator actually depend on it today** — not "might," not
   "trying to get others to use it," but genuinely using it in their own work right now?
3. **Is it already functioning like production**, even informally?

**Intent to share is not the same as actual dependents.** A project someone's proud of
and wants others to adopt is still `PLAYGROUND` until someone else actually relies on
it — "I'm trying to get people to use this" describes an aspiration, not a current
state, and should be evaluated as PLAYGROUND until that changes.

**Worked example:** A developer builds a project to learn Git, and starts encouraging
others to try it. At that point it looks ambiguous — is it a personal exercise, or
team-level work in the making? The resolving question is simple: *is anyone else
actually using it yet, or is she still trying to get traction?* If she's the only user
and it's functioning as her own notes/tool, it's `PLAYGROUND/<her-username>` —
regardless of intent to share. If and when someone else genuinely starts depending on
it, that's the moment to move it to `LABS/`, not before.

---

## Promotion path

Work is expected to move between locations as it matures — that's normal, not a sign
something was filed wrong originally:

`PLAYGROUND/<you>` → `LABS/<name>` (once it has 2+ contributors or a real audience)
→ `PRODUCTS/<name>` (once it has an owner, users, and/or a delivery target)

A repo picking up a second user, an external dependency, or a scheduled job is the
trigger to graduate it — not something to notice by accident months later.

**Labs is not a mandatory gate.** This path applies when a project's status is
genuinely uncertain at the start — a couple of people building something without yet
knowing if it becomes a real product. If instead something is a known product from day
one (formally tasked, owner assigned, delivery target known), create it directly in
`PRODUCTS/` and start development there. Don't route already-certain work through
`LABS/` first just to follow the sequence — that only adds an unnecessary migration
step. The rule of thumb:

- **Certainty exists up front** → start in `PRODUCTS/` directly
- **Certainty doesn't exist yet** → start in `LABS/`, graduate to `PRODUCTS/` once it
  earns that status

---

## Recommended descriptions

Use these when setting the **Description** field on each group/project in GitLab
(Settings > General). Each is written as **Purpose** (what this is for) and
**What goes here** (what belongs inside), so someone new to GitLab can tell at a
glance both why something exists and where to look next.

### Groups

**`ARCHIVE`**
- Purpose: Advana-required backup of pre-migration code. Read-only — not for active development.
- What goes here: One or more backup projects containing pre-migration snapshots.

**`COMMON`**
- Purpose: Shared resources used across all products, and shared enablement material for people working across TC4JLM.
- What goes here: `COMMON/guidance` (policy, standards, onboarding docs — folders: policies/, standards/, onboarding/), `COMMON/j4lm-pylib` (shared Python library), `COMMON/j4lm-business-rules` (shared business rules), `COMMON/j4lm-ci-templates` and `COMMON/j4lm-project-template` (support for new projects), `COMMON/gitlab-training` (Git/GitLab class curriculum).

**`PRODUCTS`**
- Purpose: Owned, production-bound work with real users and delivery targets.
- What goes here: `PRODUCTS/sddb` (foundation product — other products are generated off it; see its description before changing it) and `PRODUCTS/<name>` for additional products, one project per product.

**`LABS`**
- Purpose: Team-level, pre-product work — multiple contributors, not yet owned like a product.
- What goes here: `LABS/<name>`, one project per initiative. Graduates to `PRODUCTS/` once it has an owner and users.

**`PLAYGROUND`**
- Purpose: Individual, unsupported experimentation.
- What goes here: `PLAYGROUND/<username>`, one subgroup per person, holding their own project(s). Not for anything other people depend on.

**`RFIS`**
- Purpose: One-off responses to specific data requests (RFIs) — not ongoing products.
- What goes here: `RFIS/rfi-<year>-<sequence>-<short-slug>`, one project per request. Graduates to `LABS/` or `PRODUCTS/` if the same request keeps recurring.

### Projects

**`COMMON/guidance`**
- Purpose: Policy, standards, and onboarding docs for TC4JLM.
- What goes here: Markdown docs organized into folders (policies/, standards/, onboarding/), with a root index README. Start there.

**`COMMON/j4lm-pylib`**
- Purpose: Shared, versioned Python utility library.
- What goes here: Reusable functions/modules used across products, src/ layout, published to the GitLab Package Registry. Install a pinned version — don't vendor or copy-paste this code.

**`COMMON/j4lm-business-rules`**
- Purpose: Business rules shared by 2+ products.
- What goes here: Versioned rule modules, SME-reviewed, semver-tagged. Changes require sign-off — see CODEOWNERS. Not for rules that only apply to one product.

**`COMMON/j4lm-ci-templates`**
- Purpose: Shared CI/CD pipeline definitions.
- What goes here: Reusable .gitlab-ci.yml includes (lint, type-check, test, build, deploy). Include via .gitlab-ci.yml — don't duplicate stages locally.

**`COMMON/j4lm-project-template`**
- Purpose: Starter scaffold for new product repos.
- What goes here: Starter src/ layout, pyproject.toml, CI config, README template. Use when creating a new project under PRODUCTS/.

**`COMMON/gitlab-training`**
- Purpose: Git/GitLab curriculum for internal training.
- What goes here: Course material and exercise scaffolding. Stays flat under COMMON/ (not nested under a training/ subgroup) unless a second class makes a shared parent worthwhile. Live per-cohort exercise workspaces live in LABS/, not here.

**`PRODUCTS/sddb`**
- Purpose: Foundation product — other products are generated off this. Owned by its own dedicated team.
- What goes here: Core codebase, bteq/ (legacy Teradata load scripts, flagged as deprecating), business_rules/ (sddb-specific), release-tagged (semver). Downstream consumers pin to release tags, not main. Does not include validation or downstream reporting — see below.

**`PRODUCTS/sddb-validation`**
- Purpose: Validates each monthly sddb data pull; produces the go/no-go signal analysts check before using that month's data. Owned by a separate team from sddb's core codebase.
- What goes here: Schema checks (row counts, null rates, expected columns/types), business-rule checks, and a report module. Pinned to the sddb release tag it validates. Runs monthly.

**`PRODUCTS/sddb-sankey-report`**
- Purpose: Quarterly Sankey visualization from validated sddb output, for analyst use. Owned by a separate team from sddb's core codebase.
- What goes here: Extract/transform/render modules. Pinned to a specific sddb release tag. README states which sddb version and validation pass it depends on.

**`PRODUCTS/<name>`**
- Purpose: Owned, production-bound product with real users.
- What goes here: Standard src/, tests/, pyproject.toml, business_rules/ (if any product-specific rules exist), CI config. See COMMON/guidance for repo standards.

**`LABS/<name>`**
- Purpose: Team-level, pre-product work — not yet owned like a product.
- What goes here: Incubating ideas that outgrew PLAYGROUND. Graduates to PRODUCTS/ once it has an owner, users, or a delivery target.

**`LABS/gitlab-training-<year>-<cohort-id>`**
- Purpose: Live exercise workspace for one training cohort.
- What goes here: Whatever that cohort's exercises produce. Created fresh per cohort (e.g. gitlab-training-2026-q3), not reused/wiped. Archive once the session wraps.

**`PLAYGROUND/<username>/<repo>`**
- Purpose: Individual, unsupported experimentation.
- What goes here: One person's scratch work. Not maintained — nothing else should depend on this.

**`RFIS/rfi-<year>-<sequence>-<short-slug>`**
- Purpose: One-off response to a specific data request.
- What goes here: The query/script that answered the request, the delivered output (if kept in-repo), and a README recording who asked, what was asked, what was delivered, and the source data version used (e.g. sddb release tag).

---

## Exceptions

If your situation genuinely doesn't fit any row above, don't create a new top-level
subgroup on your own. Subgroup/project creation directly under `TC4JLM` is restricted
to admins for this reason. Bring it up with the GitLab admin team so the location gets
decided deliberately — that's what keeps this table accurate instead of stale.
