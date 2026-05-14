# Jira CSV Import — How to use `jira-import.csv`

**File:** [`jira-import.csv`](jira-import.csv)
**Contents:** 15 module Epics + 22 Sprint 1 Stories (6 already marked Done per Asrar's 2026-05-12 BE sync) + 1 infra story.

This file maps the [Sprint Plan §3 Sprint 1 backlog](../managerial/06-sprint-plan.md) into a Jira-compatible CSV.

---

## Step 1 — Create the Jira project (one-time)

1. Sign in to your Jira Cloud free workspace.
2. **Projects → Create project → Scrum template.**
3. Project name: **Azkashine Job Portal** (or whatever you prefer).
4. Project key: pick a short prefix, e.g. **`JOB`** — every ticket becomes `JOB-1`, `JOB-2`, etc. Hard to change later, so pick something durable.
5. Access: company-managed (gives more flexibility than team-managed for custom fields).
6. **Enable Story Points** — Project settings → Features → Story Points (default on for Scrum but verify).

## Step 2 — Verify field availability before import

In project settings, confirm these fields exist (most are default on Scrum projects):

- **Components** — the CSV uses `M1`, `M2`, … `M15` as component values. Pre-create them: Project settings → Components → Add for each of M1 through M15. **Or** let the importer create them on the fly (recommended — saves 15 clicks).
- **Story Points** — default on Scrum.
- **Labels** — default on every project; no setup needed.

## Step 3 — Run the importer

1. **Settings (cog icon) → System → External system import → CSV.**
2. Upload `jira-import.csv`.
3. Set **Date format** to whatever — the CSV has no date columns, so this doesn't matter.
4. **Project**: select the Job Portal project you just created.
5. Click **Next**.

### Field mapping (Jira importer)

Map each CSV column to the matching Jira field:

| CSV column | Jira field |
|---|---|
| Issue Type | Issue Type |
| Summary | Summary |
| Description | Description |
| Status | Status |
| Priority | Priority |
| Story Points | Story Points |
| Components | Components |
| Labels | Labels |
| Epic Name | Epic Name |
| Epic Link | Epic Link |

Most map by name automatically.

### Status mapping

The CSV uses `To Do` and `Done`. If your Jira workflow uses different status names (e.g. `Backlog` instead of `To Do`), map accordingly during the import wizard.

## Step 4 — Post-import housekeeping (~15 minutes)

The CSV intentionally **does not include Assignee** — Jira fails the import on assignees whose accounts don't exist yet. Bulk-assign after import:

1. Make sure all team members have Jira accounts on your workspace (Nazir, Asrar, Dheeraj, Najeeb, Nayan; Shaik as optional observer).
2. Open the project board.
3. Filter by label `owner-asrar` → select all → **Bulk edit → Assign to** Asrar. Repeat for `owner-nazir`, `owner-dheeraj`, `owner-nayan`.
4. Procurement-only tickets (S1-02, S1-03, S1-04) are labeled `role-procurement` — those stay with Nazir.

## Step 5 — Create the Sprint and add tickets

1. **Board → Backlog**.
2. Click **Create sprint** → name it **Sprint 1 (May 11–24)**.
3. Filter backlog by label `sprint-1` → select all → drag into Sprint 1.
4. Set **sprint start date** to 2026-05-11, **end date** to 2026-05-24.
5. **Start sprint**.

## Step 6 — Verify

Spot-check that:
- 15 Epics exist, one per module (M1–M15).
- Stories are linked to their epics (the Epic Link column matches the Epic Name).
- 6 stories are already in `Done` status (S1-01, S1-07, S1-08, S1-09, S1-10, S1-11 — Asrar's day-1 cleanup batch).
- Story Points are populated (1, 2, or 5 depending on size).
- Components show M1 through M15 on the relevant tickets.

---

## What's in the CSV vs what's not

**Included:** all Sprint 1 stories from [`06-sprint-plan.md`](../managerial/06-sprint-plan.md) §3, mapped to module Epics. Day-1 batch (S1-01 through S1-11) marked Done where Asrar confirmed completion 2026-05-12.

**Not included (intentional):**
- **Sprint 2 / Sprint 3 stories** — [Sprint Plan §4 / §5](../managerial/06-sprint-plan.md) only list these at title-level. Break them into actual backlog items at S1 close (Fri 2026-05-22), then run a second CSV import OR create them directly in Jira.
- **Section 8 open assignments** — those are operational decisions, not work items. Track them separately (status doc + scope-locked doc).
- **Bugs found during S1** — create new tickets in Jira as you find them; they don't pre-exist in the Sprint Plan.

## Connecting GitHub to Jira (recommended)

Once the project exists:

1. In Jira: **Apps → Find new apps → search "GitHub for Jira" → install** (Atlassian's official, free).
2. Connect your GitHub org / repos (both `Job_Portal` and `job-portal-backend-azka`).
3. From then on:
   - Commits like `JOB-12 fix audio cap` auto-link to the ticket.
   - Branch names like `feature/JOB-12-audio-cap` auto-link.
   - PR titles like `JOB-12: cap audio at 2 min` show up under the ticket's Development panel.
4. Smart Commit syntax also works:
   - `JOB-12 #close` — closes the ticket on merge.
   - `JOB-12 #time 2h` — logs time on the ticket.
   - `JOB-12 #comment Fixed the off-by-one` — adds a comment.

## Open assignments now living in Jira

After import, the canonical source for **task status** is Jira, not the Sprint Plan markdown. Update protocol changes:

- **Before 2026-05-14:** `06-sprint-plan.md` was the live tracker.
- **From 2026-05-14 onward:** `06-sprint-plan.md` is the **planning snapshot** that led to the tickets. Jira is the live tracker. Daily standups reference Jira.

Re-import or hand-create new tickets as S2 / S3 backlog firms up.

## If you change your mind on Jira

If Jira feels heavy after a week and you want to fall back, the same CSV imports cleanly into:

- **Linear** — File → Import → CSV (auto-detects most columns; will create a new project).
- **Asana** — paid feature on most tiers; CSV column mapping similar to Jira.
- **ClickUp** — Settings → Imports → CSV; supports the same column set.
- **GitHub Issues** — no native CSV import; would need a script (`gh issue create` in a loop reading the CSV). ~30 min of `bash` work.

But Jira is the right pick for a 5-engineer / 6-week build with a dedicated QA — don't overthink it.
