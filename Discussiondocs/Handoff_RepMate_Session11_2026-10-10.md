# Handoff — RepMate — Session 11 (2026-10-10)

**Handoff created by:** Karthik + Claude
**Project:** RepMate (gym tracker PWA — `kwin5786/repmate`, personal GitHub, NOT the iLenSys org)
**Track:** Single track — no parallel tracks.
**Branch:** `main`
**Code position:** `820d934` — **unchanged this session.** No application code was written; `index.html` was not touched. Local and remote HEAD verified identical via `git ls-remote origin main`.

---

## 1. Progress table

| # | Item | Phase / Module | Status | Last Touched | Notes |
|---|---|---|---|---|---|
| 1 | Stack decision (Supabase + GitHub Pages + UptimeRobot, ₹0) | Foundation | ✅ Completed | 2026-08-19 | ₹0 rule HARD |
| 2 | Supabase project (Mumbai) + 5 tables + storage buckets | Database | ✅ Completed | 2026-10-10 | No schema change; constraints now documented |
| 3 | GitHub repo `kwin5786/repmate` (public) + Pages hosting | Hosting | ✅ Completed | 2026-08-19 | Live at kwin5786.github.io/repmate |
| 4 | v1 app: login + Today tab logging | App v1 | ✅ Completed | 2026-08-19 | Login superseded by item 21 |
| 5 | Logo (two-tone) live on login screen | Branding | ✅ Completed | 2026-08-19 | logo.png, commit f1dc950 |
| 6 | UptimeRobot keep-alive monitor | Ops | ✅ Completed | 2026-08-19 | Still green |
| 7 | Timed sets (multi-set s/m, canonical secs, legacy compat) | App v2 | ✅ Completed | 2026-08-19 | commit 357d682 |
| 8 | Date-picker button fix | App v2 | ✅ Completed | 2026-08-19 | Same commit |
| 9 | appicon.png committed | App v2 | ✅ Completed | 2026-08-19 | commit bfce1c6 |
| 10 | History tab (day cards + calendar strip + SVG charts) | App v2 | ✅ Completed | 2026-08-19 | commit f91c537 |
| 11 | Profile tab (onboarding, stats, BMI, body-weight chart) | App v3 | ✅ Completed | 2026-08-20 | commit ba381b9 |
| 12 | Calorie burn per workout (MET-based, display-only) | App v3 | ✅ Completed | 2026-08-20 | commit cde9549 |
| 13 | PWA manifest + appicon + apple-touch-icon | App v2 | ✅ Completed | 2026-08-20 | commit 53a7cb9 |
| 14 | Groups (create/join + group summary/compare) | App v3 | ❌ Cancelled | 2026-10-09 | Replaced by item 23 |
| 15 | Admin — member slice (list, add member, PIN reset) | App v3 | ✅ Completed | 2026-08-20 | commit 51ead14 |
| 16 | Progress photos (front/side/back, private, compare dates) | App v4 | ⏳ Planned | 2026-10-10 | Table empty (0 rows). `pose` CHECK = front/side/back |
| 17 | Exercise visuals — thumbnail + detail view | App v4 | 🟡 In Progress | 2026-10-10 | **6 of 64 images live and wired.** Style prompt + host settled. Display code NOT built |
| 18 | Android APK via PWABuilder (sideload, NO store push) | Later | ⏳ Planned | 2026-08-20 | iOS native permanently out |
| 19 | Weight+Time combo type (farmer's walk) | Later | ⏸ On Hold | — | Workaround: log as Time, weight in name |
| 20 | Test-data cleanup | Housekeeping | ⏳ Planned | 2026-10-10 | **LAST ROLLOUT BLOCKER.** `Backtest` deleted. Remaining: Test1, Test2, `zzz`, 18 Aug Plank row, 9 Oct Bench Press row |
| 21 | Login redesign (typed name + PIN, no member list) | App v3 | ✅ Completed | 2026-10-09 | commit 7a547bc |
| 22 | Admin — exercise rename/merge/delete | App v3 | ✅ Completed | 2026-10-10 | commit 820d934. Used in anger this session |
| 23 | Friends tab (all members' weekly summaries + compare) | App v3 | ✅ Completed | 2026-10-09 | commit 3709427 |
| 24 | Distance measure type (km) | App v3 | ⏳ Planned | 2026-10-10 | **Re-rated this session: blocked by a CHECK constraint change, not just app code** |
| 25 | Body stats in Friends + per-person hide toggle | App v3 | ✅ Completed | 2026-10-10 | commit 4eefcc5 |
| 26 | Self-test blocks for the maths | Quality | ⏳ Planned | 2026-10-09 | Calories, BMI, best-set, duration format, chart scaling, week bucketing |
| 27 | History pagination | Perf | ⏳ Planned | 2026-10-09 | Currently fetches every row ever logged |
| 28 | Parked ideas (gym layer, trainer role, invite codes, subscription dates, diet plan, assigned workouts) | Parked | ⏸ On Hold | 2026-10-09 | Deliberately set aside |
| 29 | AI integration | — | ❌ Cancelled | 2026-10-09 | Metered cost + key in a public file |
| 30 | Themed in-app modals replacing native browser dialogs | App v3 | ✅ Completed | 2026-10-09 | commit af2f071. Sweep CLOSED |
| 31 | "Developed by Karthik" credit on login screen | Branding | ✅ Completed | 2026-10-09 | commit 6229be6 |
| 32 | Manual Supabase backup before rollout | Ops | ✅ Completed | 2026-10-10 | **First ever.** 4 CSVs + schema.md + constraints.md, row counts verified |
| 33 | Rollout message (install steps per platform) | Rollout | ⏳ Planned | 2026-10-09 | iPhone: Safari → Share → Add to Home Screen. Android: install prompt |
| 34 | Seed the exercise library | Content | ✅ Completed | 2026-10-10 | **5 → 64 exercises across 9 groups, zero NULLs** |
| 35 | Grouped + searchable exercise picker | App v4 | ⏳ Planned | 2026-10-10 | **New this session.** Fixed 9-group dropdown; typed search. Code — needs survey |
| 36 | "Plan Today" empty-state + warm-up toast | App v4 | ⏳ Planned | 2026-10-10 | **New this session.** Multi-select groups; once-a-day toast. Code — needs survey |

**Legend:** ✅ Completed · 🟡 In Progress · ⏳ Planned · ⏸ On Hold · ❌ Cancelled

---

## 2. Status board

| Track | Branch | Last commit | Last session | Next step |
|---|---|---|---|---|
| RepMate (single) | `main` | `820d934` (unchanged — no code this session) | Session 11, 2026-10-10 | Item 20 (test cleanup) → item 33 (rollout) → items 35/36 |

Shared files touched: no — single-owner project, single file (`index.html`), untouched this session.

---

## 3. State + what shipped this session

A content and operations session, not a code session. Three items closed, one started, two new items created. Two rollout blockers cleared; one remains.

**Item 32 — first-ever Supabase backup. ✅**
Saved to `D:\Active Projects\Productive Apps\_backups\repmate\2026-10-10\`:
`members.csv` (4 rows), `exercises.csv` (6), `workout_logs.csv` (4), `body_weights.csv` (1), plus `schema.md` and `constraints.md`. `progress_photos` skipped — 0 rows, nothing to export; its structure is captured in `schema.md`. Every CSV row count was verified against a `count(*)` query, not against the dashboard's row estimate.

Route chosen: per-table CSV export from the Table Editor, **not** `pg_dump`. Rationale in §4. What it does **not** cover, recorded honestly: RLS policies, storage-bucket files, and project settings.

**Naming convention settled. ✅**
Four rules: Title Case every word; equipment first then movement; no abbreviations ever; plural only where the movement is inherently counted in plural (`Push Ups`, `Pull Ups`). Applied to the existing library: `Backtest` deleted (0 logs), and three renames landed — `Barbell Curl2` → `Barbell Curl`, `Bench Press` → `Barbell Bench Press`, `Triceps Pushdown` → `Cable Triceps Pushdown`.

**Item 34 — library seeded. ✅**
59 rows inserted plus 5 group backfills, taking the library from 5 to **64 exercises**. Verified by group count, predicted before looking and matched exactly: Arms 11, Back 6, Cardio 8, Chest 9, Core 9, Full Body 2, Glutes 6, Legs 7, Shoulders 6. No NULL group. Largest single write this database has taken.

Scope grew from the planned ~30 because Karthik supplied reference photographs of his actual gym, which showed a fuller equipment set than the backlog assumed.

**Item 17 — started. 🟡**
Six line-art images converted to WebP (620×620, 29–44 KB each), uploaded to `https://digital.ilensys.com/ilensys/rm/`, and `image_url` set on the matching six rows: Barbell Bench Press, Lat Pulldown, Dumbbell Shoulder Press, Leg Extension, Leg Curl, Pec Deck Fly. Verified by query — 6 rows with non-null `image_url`.

**Nothing is visible in the app yet.** Rendering `image_url` is `index.html` work and was deliberately not started.

**Items 35 and 36 — new, scoped, not built.**
Both emerged from a design conversation about making the 64-exercise library usable. Item 36's scope was materially simplified by Karthik mid-discussion (see §4).

---

## 4. Locked decisions made this session

| Decision | Rationale |
|---|---|
| Backup is per-table CSV export + a schema/constraints snapshot, **not** `pg_dump` | `pg_dump` needs Postgres client tools installed and a connection string on the command line — a new install and a new failure mode on the day whose purpose is reducing risk. The realistic failure was "I seeded junk into `exercises`", which a CSV fully covers. A backup that is easy to repeat beats a perfect one done once |
| Naming convention: Title Case, equipment-first, no abbreviations, plural only where inherent | A near-empty library is how "Benchpress"/"BP"/"bench press" happens. The plural rule is the one genuinely arbitrary part and was called as such |
| Nine muscle groups: Chest, Back, Shoulders, Arms, Legs, Glutes, Core, Cardio, Full Body | Eight were proposed; Glutes was split out because the reference photos yielded 6 glute exercises and folding them into Legs made a 13-item list that defeats the purpose of grouping |
| Biceps and Triceps stay **merged** as Arms, despite the source photos splitting them | 11 items is long but scannable. Splitting forces an "is this biceps or arms?" decision on every future add — one more chance to get it wrong |
| `muscle_group` must be a **fixed dropdown**, never free text | Unlike `measure_type`, `muscle_group` has NO database constraint. It will accept `Legs`, `legs`, `Leg` and `LEGS` as four separate groups. The database will not stop drift; only the UI can |
| Standard gym equipment assumed where the photos were silent (squat rack, leg press, calf machine, incline bench, cable crossover) | Karthik confirmed "mostly standard equipment". Anything absent is two taps to delete at 0 logs — cheaper than discovering a missing exercise mid-session |
| Image filename always matches the exercise name, lowercased and hyphenated | `Butterfly.jpg` was stored as `pec-deck-fly.webp` because the exercise is seeded as `Pec Deck Fly`. The filename follows the database, not the source file, so `image_url` can be derived mechanically rather than via a lookup table |
| Image specs: square 1:1, 620×620, WebP, under 60 KB, one fixed style prompt | Consistency across 64 images is what makes a set look like a set. The style prompt is recorded in §5 so image 50 matches image 1 |
| Item 17 ships **gradually**, decoupled from rollout | 64 images at 3–5 min each is 4–6 hours. `image_url` is nullable, so an exercise without an image works perfectly. Once people are using it, the ~15 most-logged exercises are the ones worth the effort |
| Item 36 reduced from a multi-step wizard to a single empty-state change | Karthik's simplification, not Claude's: "Plan Today" appears only when the day has no logs; Cardio is simply one of the nine selectable groups rather than a separate step; the warm-up message is a once-a-day toast. This collapsed four stacked features into one item |
| Warm-up message is a 5-second toast with modest, non-prescriptive wording | "Warm up before you start — a few light sets beat a cold first rep." The app is not a trainer. Static stretching before lifting is contested, and advice given to colleagues should not imply the app knows what prevents injury. 2–3s was too short to read |

---

## 5. Next session — focus, steps, and files

**Session 12 target: item 20 — the last rollout blocker.** All deletes, no code. Remaining: `Test1` and `Test2` members, `zzz`, the 18 Aug Plank log row, the 9 Oct Bench Press log row.

**One decision to make first:** those two log rows are the only real workout data in the database. Deleting them leaves History and Friends empty — correct for a clean rollout, but it also removes the only data available for eyeballing items 35 and 36 during development.

**Then item 33** (rollout message), **then items 35 and 36** — both are `index.html` changes and must not be proposed before Claude Code reads the file verbatim.

**Known before the survey, from this session's database work:**
- `measure_type` CHECK permits exactly `weight`, `time`, `reps`
- `muscle_group` has no constraint — the dropdown is the only guard
- `exercises.name` and `members.name` are both UNIQUE
- `exercises.image_url` and `video_url` already exist; item 17 needs no schema change

Pre-chat:

```powershell
cd "D:\Active Projects\Productive Apps\repmate"
$out = (git pull 2>&1 | ForEach-Object { "$_" }) -join "`n"
$out | Set-Clipboard
$out
```

Local test server for verification (leave the terminal alone while testing; Ctrl+C to stop):

```powershell
cd "D:\Active Projects\Productive Apps\repmate"
python -m http.server 8080
```

Git close-out — three separate blocks, never chained:

```powershell
git add index.html
```

```powershell
git commit -m "Session 12: short description"
```

```powershell
$out = (git push 2>&1 | ForEach-Object { "$_" }) -join "`n"
$out | Set-Clipboard
$out
```

**File Gather for next session:**

```powershell
$Repo = "D:\Active Projects\Productive Apps\repmate"
$Dest = "D:\Active Projects\Productive Apps\_drop"
$files = @(
  "index.html"
)
New-Item -ItemType Directory -Force -Path $Dest | Out-Null
Get-ChildItem -Path $Dest -File -ErrorAction SilentlyContinue | Remove-Item -Force
foreach ($f in $files) {
  $src = Join-Path $Repo $f
  if (Test-Path $src) {
    $flat = ($f -replace '[\\/]', '__')
    Copy-Item $src (Join-Path $Dest $flat) -Force
    "copied: $flat"
  } else {
    "NOT FOUND: $f"
  }
}
Start-Process explorer.exe $Dest
```

**Image style prompt (locked — use verbatim for every remaining image):**

```
Minimalist line-art illustration of a muscular male figure performing
[EXERCISE NAME] on standard gym equipment. Pure black background.
Thin white outline strokes only, no shading or fill on the body.
The primary worked muscle is filled in bright lime green (#C6F432) as
the only colour in the image. Equipment drawn in thin grey outline.
Side or three-quarter view showing the full movement position clearly.
Square 1:1 composition, figure centred, generous margin.
No text, no logo, no watermark.
```

Two refinements to fold in next time: keep the background tone identical across images (`barbell-bench-press` came out slightly lighter than the other five), and highlight only the primary muscle (`dumbbell-shoulder-press` highlighted biceps alongside delts).

Testing trigger: no suite yet. Self-tests (item 26) remain the next quality step.

---

## 6. New lessons

1. **A row-count estimate is not a row count.** The first survey query returned `-1` for four tables and `10` for `workout_logs`. `-1` means Postgres has never run `ANALYZE`; the `10` was a stale guess against a true count of 4. **Must not repeat:** when a number will be used as a verification baseline, get it from `count(*)`. An authoritative-looking screen is not evidence.

2. **CHECK constraints can silently re-rate a backlog item.** `exercises_measure_type_check` permits only `weight`, `time`, `reps`. Item 24 (distance in km) was listed as an app feature; it actually requires a constraint change on shared data first. This was found by reading constraints during a backup, not during planning. **Must not repeat:** read the constraints before estimating any item that writes new kinds of value — Session 10's lesson 2 has now cost something twice.

3. **The absence of a constraint is as important as its presence.** `measure_type` is guarded by the database; `muscle_group` is not, and will happily store four spellings of "Legs". Both facts came from the same query, and only one was expected. **Must not repeat:** when surveying constraints, record what is *unguarded*, because that is where the UI has to do the work.

4. **"Done" needs a destination before it can be verified.** Three rounds were lost to a loop: a report of "done", a query showing nothing changed, and a re-send of the same instructions. The cause was an assumption that the renames had been attempted when the exercise in question had in fact been renamed as a deliberate test of the rename tool. **Must not repeat:** when a verification contradicts a report twice, stop re-sending the instruction and ask what actually happened on screen — the gap is in the shared picture, not in the work.

5. **Reference material is not licensed material.** The source collages used to decide the exercise list carried a visible third-party watermark. Using them to determine *which exercises exist and what to call them* is fine; shipping them inside the app would not have been. **Must not repeat:** separate "what do I learn from this" from "what may I ship", and say so at the point the files arrive rather than after they are converted.

---

## 7. Open / parked backlog

- **Remaining rollout blocker:** item 20 (Test1, Test2, `zzz`, 18 Aug Plank row, 9 Oct Bench Press row), then item 33 (rollout message).
- **Then:** items 35 (grouped search) and 36 (Plan Today) — both code, both need a verbatim survey of `index.html`; item 17's remaining 58 images; item 24 (distance in km — now a schema change first); item 26 (self-tests — BMI and calories are where a silent arithmetic error would be believed rather than noticed).
- **Hosting decision to revisit:** the six images are served from `digital.ilensys.com`, which is iLenSys infrastructure, while RepMate is explicitly a personal project. The directory is also openly browsable. The repo alternative is ~2.3 MB for all 64, free on GitHub Pages, and keeps assets with the code. Changing later is one `update` statement across `image_url`.
- **Known soft spots, carried:** merge is sequential, not atomic; a failed delete after a successful sets-write can double-append one day's sets on re-run (documented in a code comment, accepted); loser-loser same-day duplicates are preserved rather than repaired; the rename duplicate error echoes the typed name rather than the existing exercise's actual name (cosmetic); Friends week headings recompute at render; the Friends `body_weights` query has no date floor and meets Supabase's 1000-row cap eventually.
- **Parked ideas (item 28):** gym entity, trainer role, WhatsApp invite codes, subscription end dates, trainer-written diet plans, trainer-assigned workouts.
- **Deferred quality:** full browser check suite (needs a `_test` Supabase project and an overridable URL in `index.html`); History pagination (item 27).
- **Backup cadence:** the 2026-10-10 backup is a point-in-time snapshot, not a schedule. Worth repeating before any future bulk write.

---

## 8. Closing pause

No code was written this session, and it was still one of the more consequential ones. The database went from a near-empty test harness to a real library: 64 correctly-named exercises across nine groups, every one conforming to a convention that now exists, with a verified restore point sitting on disk underneath it. The first backup in the project's life was taken before the largest write in the project's life, in that order, deliberately.

The thing that earned its keep was reading constraints. Two of this session's five lessons came from a query run for backup purposes that happened to reveal what the database will and will not accept — and one of them quietly made item 24 a bigger job than the backlog claimed. The other half of that finding is subtler: `muscle_group` has no guard at all, which is precisely why item 35 must offer a dropdown rather than a text box. A database that accepts anything is a design constraint on the UI, not a convenience.

What matters most next time: item 20 is the last blocker, and it is five deletes. Decide first whether to keep the two real log rows for development visibility, because once they are gone the app has nothing to render while items 35 and 36 are being built. After that it is a rollout message and the app is in people's hands — which is the point at which the 58 missing images start to matter, and not before.

---

## Kickoff block for next session

```
Resuming a working session on RepMate.
Handoff attached: Handoff_RepMate_Session11_2026-10-10.md
Branch: main (SOLO — single-owner project, personal GitHub kwin5786/repmate, NOT iLenSys).
Status: Items 32 and 34 shipped — backup taken, library seeded to 64 exercises across 9 groups. Item 20 is the last rollout blocker.

Standing rules apply (they live in memory — do not re-list them).

Opening ritual, in order:
1. Ask which machine I'm on today (ASUS or Mac — the repo exists only on ASUS at D:\Active Projects\Productive Apps\repmate).
2. Read this handoff fully.
3. Under "Here's where the project stands:", show the progress table and the status board from the handoff FIRST, before any prose.
4. Give me the git pull command for my machine (PowerShell block, output piped to clipboard). Wait for my confirmation that the pull is clean. Expect HEAD at or ahead of 820d934 on main.
5. Survey first: Claude Code reads index.html verbatim before proposing anything — never guess function names or structure. Bring ONE recommendation, then WAIT for my explicit go.

Queued, in this order: item 20 (delete Test1, Test2, zzz, the 18 Aug Plank row, the 9 Oct Bench Press row), then item 33 (rollout message), then items 35 and 36.

Decide before item 20 starts: whether to keep the two real workout log rows for development visibility, since deleting them leaves History and Friends empty while 35 and 36 are built.

Verification rule: every step gets a clickable link and literal click-by-click instructions. For destructive actions on shared data, confirm in BOTH the app and a database query — the two are separate claims. Row counts come from count(*), never from a dashboard estimate.
```
