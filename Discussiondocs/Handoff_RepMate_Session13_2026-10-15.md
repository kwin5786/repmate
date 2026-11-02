# Handoff — RepMate — Session 13 (2026-10-15)

**Handoff created by:** Karthik + Claude
**Project:** RepMate (gym tracker PWA — `kwin5786/repmate`, personal GitHub, NOT the iLenSys org)
**Track:** Single track — no parallel tracks.
**Branch:** `main`
**Code position:** `035d6e3` — one commit this session. Push verified: remote moved `e3e36fc..035d6e3`. No uncommitted work; the library cleanup was database-side, not code.

> **Note:** this handoff supersedes nothing in `Handoff_RepMate_Session12_2026-10-11.md` — it continues it. Session 12's handoff was written mid-session and does not cover item 37 (admin search/sort/serial numbers), the library cleanup, or the calorie and nutrition studies. Read both.

---

## 1. Progress table

| # | Item | Phase / Module | Status | Last Touched | Notes |
|---|---|---|---|---|---|
| 16 | Progress photos | App v4 | ⏳ Planned | 2026-10-10 | Table empty |
| 17 | Exercise visuals — thumbnail + detail view | App v4 | 🟡 In Progress | 2026-10-15 | Display built. 6 of 85 wired; the rest render the lettered placeholder |
| 18 | Android APK via PWABuilder | Later | ⏳ Planned | 2026-08-20 | iOS native permanently out |
| 19 | Weight+Time combo type | Later | ⏸ On Hold | — | Workaround in place |
| 20 | Test-data cleanup | Housekeeping | ✅ Completed | 2026-10-11 | Closed in Session 12 |
| 24 | Distance measure type (km) | App v3 | ⏳ Planned | 2026-10-10 | Needs a CHECK constraint change. **May be absorbed by item 42** |
| 26 | Self-test blocks for the maths | Quality | ⏳ Planned | 2026-10-09 | Now more relevant — item 42 rewrites the calorie maths |
| 27 | History pagination | Perf | ⏳ Planned | 2026-10-09 | Fetches every row ever logged; 5 members now writing daily |
| 28 | Parked ideas | Parked | ⏸ On Hold | 2026-10-09 | Gym layer, trainer role, invite codes |
| 29 | AI integration | — | ❌ Cancelled | 2026-10-09 | Metered cost + public key |
| 33 | Rollout message | Rollout | ✅ Completed | 2026-10-11 | 5 members onboarded in person |
| 34 | Seed the exercise library | Content | ✅ Completed | 2026-10-10 | 64 seeded; now 85 after member additions |
| 35 | Grouped + searchable exercise picker | App v4 | ✅ Completed | 2026-10-11 | commit c2a2bc4 |
| 36 | "Plan Today" empty state + warm-up toast | App v4 | ✅ Completed | 2026-10-11 | commit e611810 |
| 37 | Admin exercises — search, sort, serial numbers | Admin | ✅ Completed | 2026-10-15 | **commit 035d6e3.** Shipped this session |
| 38 | Avatar → Profile nav, drop Profile tab | App v4 | ⏸ On Hold | 2026-10-11 | K avatar still not clickable |
| 39 | Timer for timed sets | App v4 | ✅ Completed | 2026-10-11 | commit e3e36fc |
| 40 | Member-changeable PIN | App v4 | ⏳ Planned | 2026-10-11 | All four new members still on `0000` |
| 41 | Member edit screen — rename, PIN reset, delete | Admin | ⏳ Planned | 2026-10-11 | Fully specified in Session 12 §5. Member admin still has only Add friend + Reset PIN |
| 42 | Workout detailing + calorie accuracy | App v4 | ⏳ Planned | 2026-10-15 | **New. Next session, as a STUDY not a build.** See §3 and §5 |
| 43 | Nutrition & calorie tracker module | App v5 | ⏳ Planned | 2026-10-15 | **New.** Scoped this session; depends on item 42. See §3 |
| 44 | Library hygiene — duplicate merges | Content | 🟡 In Progress | 2026-10-15 | **New.** Squat trio resolved; 3 pairs still undecided. See §7 |

Items 1–15, 21–23, 25, 30–32 remain ✅ Completed and unchanged.

**Legend:** ✅ Completed · 🟡 In Progress · ⏳ Planned · ⏸ On Hold · ❌ Cancelled

---

## 2. Status board

| Track | Branch | Last commit | Last session | Next step |
|---|---|---|---|---|
| RepMate (single) | `main` | `035d6e3` | Session 13, 2026-10-15 | Item 42 (workout detailing + calorie accuracy) — design document first, no code |

Shared files touched: no — single-owner project, single file (`index.html`).

---

## 3. State + what shipped this session

Four days of real use by five people. The app is no longer a build project — it is a live tool with daily writes from four colleagues plus Karthik.

**Item 37 — Admin exercises: search, sort, serial numbers. ✅ `035d6e3`**
The admin exercise screen was an unsorted 85-row wall. It now has a sticky bar with a search box, three sort chips (Name / Muscle group / Most logged), a count line (`85 exercises` or `4 of 85`), and a serial number on every row that renumbers over the filtered, sorted array. All render-layer — no new query, no schema change. The one real risk was the rename/merge/delete handlers pointing at the wrong exercise after reordering; they bind to the `ex` object from the forEach, with the index used only for the number, and this was read back from the file and then verified in the app by renaming row 4 of a filtered list.

**Library drift — found and partly fixed.**
In four days the library went 64 → 86 exercises, roughly five new ones a day. The new entries were inconsistent in ways the seeded ones were not: `Squad` had a **null muscle group** (the defensive "Other" bucket from item 35 actually firing), duplicated `Squads`, and both overlapped the seeded `Barbell Back Squat`. Casing had split from Title Case (`high-pulley cable fly`, `Dumbbell shrugs`). Resolved this session: `Squads` renamed to `Dumbbell Squat`, `Squad` merged into it (2 logs preserved), `Leg raise abs` renamed to `Lying Leg Raise`. Library now **85 exercises**. Remaining casing fixes were deliberately dropped as not worth the time — search is case-insensitive and nothing depends on them.

**Calorie accuracy — diagnosed, not fixed. This is item 42.**
Karthik flagged that a real session (8 exercises, 24 sets) reported ~70 kcal. Reading `logCalories()` found the cause: for non-time exercises, `secs = sets.length * SET_SECONDS`, which is **40 seconds per set and nothing else**. Rest between sets is not counted at all, so a 50-minute session is scored as 16 minutes — roughly a 3× undercount. Second problem: `MET_BY_TYPE` assigns a single value of 3.5 to every weight exercise, so a standing calf raise and a bench press score identically. Time-based exercises are correct (Treadmill Walk 5m → 20 kcal checks out against 3.5 MET × 68 kg).

**Key finding for item 42, from auditing all 49 logged exercises:** per-exercise MET values are the **wrong fix for 43 of them**. Published MET tables barely distinguish resistance exercises. Cardio is the opposite — speed and incline are exactly what MET tables are built on, and the four logged cardio exercises (Treadmill Walk, Treadmill Run, Stationary Cycling, EFX) would become genuinely accurate with those two fields. So the design splits: **parameters for cardio, a volume-based model for weights**.

**Item 43 — nutrition & calorie tracker: scoped, nothing built.**
Karthik brought a full requirements list (daily target, macro logging, South Indian and homemade foods, raw/cooked, favourites, waist tracking, export). Architecture review: the existing stack carries it — same Supabase client, same sheet pattern, `body_weights` already tracks daily weight. Three new tables (`foods`, `food_logs`, waist added to `body_weights`). **The hard constraint is food data, not code.** USDA FoodData Central is public domain and excellent but near-useless for Indian home cooking; Open Food Facts is openly licensed but barcoded packaged products only; IFCT 2017 (National Institute of Nutrition) is the authoritative Indian source but **is not openly licensed**, so it can inform a hand-curated list, not a bulk import. Conclusion: Karthik seeds his own ~120–150 food library, the way the 64 exercises were seeded. Claude will not generate nutrition values — that would be inventing data.

---

## 4. Locked decisions made this session

| Decision | Rationale |
|---|---|
| Item 37 sort options are exactly three: Name, Muscle group, Most logged | "Muscle group" is the duplicate-finder (variants land adjacent); "Most logged" is the priority list for item 42. Least-logged and recently-added are derivable from these and cost screen space |
| Serial numbers follow the sort, renumbering 1..N over the filtered list | Their value is calling out a row ("number 47 is a duplicate"), not counting. A fixed number would break that |
| Zero-result filter renders "No exercises match.", not the 🏋️ empty state | The empty state means "the library is empty" and would be misleading mid-search |
| Casing cleanup abandoned as not worth the time | Search is case-insensitive; nothing functional depends on it. Duplicates matter, capitals do not |
| `Squads` → `Dumbbell Squat`, with `Squad` merged in; 0 kg means bodyweight | Karthik uses dumbbells and logs 0 kg when going bodyweight. Explicitly NOT merged into `Barbell Back Squat` — different load, merging would corrupt the history |
| `Leg raise abs` → `Lying Leg Raise` | Karthik's correction of the actual movement name |
| Per-exercise MET values rejected as the model for resistance work | Published MET tables barely distinguish resistance exercises. Burn is driven by load moved, time under tension and rest |
| Cardio and weights get **different** calorie models | Speed/incline parameters for cardio; a volume-based model for weights. One model cannot serve both |
| Item 43 (nutrition) will NOT consume workout calorie estimates | Standard practice is an activity multiplier in the daily target. Feeding workout estimates in double-counts and makes the deficit depend on a number known to be wrong |
| Nutrition food data: Karthik hand-seeds the library; Claude generates no nutrition values | IFCT 2017 is not openly licensed; USDA and Open Food Facts do not cover South Indian home cooking. Inventing values is worse than having none |
| Supabase writes stay manual (dashboard); a read-only connection for Claude Code is worth setting up later | Read-only removes the copy-paste survey loop at contained risk. Writes keep the human gate that caught the two unaccounted 20 Aug rows in Session 12 |
| Session 12's handoff is continued, not replaced | It was written mid-session and is accurate for what it covers; this one carries everything after it |

---

## 5. Next session — focus, steps, and files

**Session 14 target: item 42 (workout detailing + calorie accuracy). This is a STUDY session — a design document before any code.**

What the document must settle:

- **Cardio model.** Which fields each cardio exercise needs (speed, incline, resistance level), how they map to MET values, and where those MET values come from. Cite real sources (the Compendium of Physical Activities is the standard reference) — do not invent values.
- **Resistance model.** A volume-based calculation using load moved, reps, set count and realistic inter-set rest. Must handle 0 kg as bodyweight, not as zero work.
- **Reps/bodyweight model.** Four logged exercises sit here; closest to the resistance model.
- **Schema changes.** New columns on `exercises` and/or a parameters object on the `sets` jsonb. Note that `measure_type` has a CHECK constraint — item 24 (distance in km) is blocked on the same constraint and may be worth absorbing into this work.
- **Migration.** 85 exercises and ~90 existing log rows already exist. What happens to historical calorie figures when the model changes.

**Data hazards the design must handle (found in this session's audit):**

1. **The same person logged Standing Calf Raise at 0 kg, 10 kg and 110 kg across three days** — different machines under one exercise name. A naive `kg × reps` volume model would let one person's 110 kg calf raise dwarf an entire session. This is the strongest argument for recording the machine/setup, not just the exercise name.
2. **0 kg entries mean bodyweight**, and multiply to zero under any volume model.
3. Similar spread on Leg Press and Lat Pulldown across different members.

**Then:** item 43 (nutrition & calorie tracker), then item 40 (member-changeable PIN) and item 41 (member edit screen) — Karthik explicitly deprioritised 40 and 41 behind the calorie and nutrition work.

Pre-chat:

```powershell
cd "D:\Active Projects\Productive Apps\repmate"
$out = (git pull 2>&1 | ForEach-Object { "$_" }) -join "`n"
$out | Set-Clipboard
$out
```

Local test server (leave the terminal alone while testing; Ctrl+C to stop):

```powershell
cd "D:\Active Projects\Productive Apps\repmate"
python -m http.server 8080
```

Git close-out — three separate blocks, never chained:

```powershell
git add index.html
```

```powershell
git commit -m "Session 14: short description"
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

Testing trigger: no suite yet. Item 26 (self-tests for the maths) becomes materially more valuable once item 42 rewrites the calorie calculation — worth pairing them.

---

## 6. New lessons

1. **A handoff written mid-session goes stale the moment work continues.** Session 12's handoff was written at what looked like a stopping point; four more hours of work followed, and none of it — item 37, the library cleanup, the calorie diagnosis, the nutrition study — was in that file. **Must not repeat:** when work resumes after a handoff, the handoff is no longer current; re-run the closing ritual at the actual end, and note in the new handoff that it continues rather than replaces the previous one.

2. **A library with no entry discipline drifts fast under real users.** 22 new exercises in four days, including a null muscle group, a typo duplicate, and split casing. The defensive "Other" bucket built in item 35 caught the null — that guard earned itself in under a week. **Must not repeat:** when a shared library is opened to multiple users, expect drift at roughly 5 entries/day/user and plan the hygiene tooling (search/sort/merge) before rollout, not after.

3. **Verify a feature's premise before specifying its fix.** The question "how do I delete a user who leaves?" was answered correctly only because `renderAdminList` was read first — the member admin has **only** Add friend and Reset PIN, no rename and no delete, despite an older memory note saying "add/remove". **Must not repeat:** check the actual code before describing what a screen can do, even for a screen that shipped sessions ago.

4. **The user is the limiting factor on data-seeding work, and that must be said plainly up front.** The nutrition module's blocker is not architecture — it is that ~150 South Indian food entries have to be typed by a human with a trustworthy source. Saying so before scoping saved a design built on a data source that does not exist. **Must not repeat:** when a feature depends on data that cannot be generated or legally imported, surface that constraint before the architecture discussion, not after.

---

## 7. Open / parked backlog

- **Item 44 (library hygiene) — three duplicate pairs still undecided**, all needing Karthik's judgement on whether they are the same movement, all merges that preserve logs:
  - `Straight Bar Tricep` (2 logs) vs seeded `Cable Triceps Pushdown` (1 log)
  - `Pullover Chest` (2 logs, filed Chest) vs `Cable Lat pullover` (1 log, filed Back)
  - `Double D-Handle Lat Pulldown` (1 log) vs `Lat Pulldown` (4 logs)
- **Item 40 (member-changeable PIN):** all four new members share PIN `0000`, names are visible on the Friends tab, and only the admin can reset. Karthik has deprioritised this behind items 42 and 43 — it is a known, accepted, open gap.
- **Item 41 (member edit screen):** fully specified in Session 12 §5. Until it ships, removing a departing member is a Supabase dashboard delete that cascades their logs, body weights and photos away permanently (Karthik has confirmed hard delete with no archive is what he wants).
- **Read-only Supabase connection for Claude Code** — agreed as worthwhile, deliberately not set up at the end of a long session. Needs a separate Supabase role, a decision on where the key lives, and confirmation it is gitignored.
- **Backup cadence:** a fresh exercises + workout_logs export was taken this session before the merges. Worth repeating before item 42's schema changes.
- **Item 38 (avatar → Profile nav):** the K avatar is still not clickable; Karthik wants it to open Profile, with a view to dropping the Profile tab and freeing a bottom-bar slot.
- **Item 17 (exercise images):** 6 of 85 wired. Hosting is still `digital.ilensys.com` (iLenSys infrastructure on a personal project) — repo alternative is ~2.3 MB, free on Pages, one `update` to switch.
- **Thumbnails are 44px** — the lime muscle highlight is not readable at that size. Consider 56px.
- **`saveEntry` replaces the whole sets array per member+exercise+date.** With the timer making a set one tap, stray short entries are cheap to create (a 2-second accidental run appeared during Session 12 testing).
- **Known soft spots, carried:** merge is sequential, not atomic; same-day merges append both sets into one row; Friends `body_weights` query has no date floor and will meet Supabase's 1000-row cap eventually; History fetches every row ever logged (item 27).

---

## 8. Closing pause

Four days of real use taught more than the four sessions that preceded it. The app works — five people are logging daily, nobody has reported a bug — but the data coming out of it has revealed two things that building in isolation never would. The library drifts fast when real people add to it, and the calorie numbers are wrong by about three times, because the maths counts only the seconds you are actually lifting and nothing in between.

Item 37 (admin search, sort and serial numbers) shipped today and makes the first problem manageable. The second is next session's work, and it is a study rather than a build: cardio and resistance training need genuinely different calorie models, and the honest version of that needs real sources rather than a plausible-looking formula.

What matters most next time: **resist the urge to patch the calorie number.** A single constant change would make it look right and still be wrong. Do the design document, cite the sources, and decide what happens to the 90 log rows already in the database before touching `logCalories()`.

---

## Kickoff block for next session

```
Starting a fresh session on RepMate — item 42 (workout detailing + calorie accuracy).
Handoff attached: Handoff_RepMate_Session13_2026-10-15.md (continues, does not replace, Handoff_RepMate_Session12_2026-10-11.md).
Branch: main (SOLO — single-owner project, personal GitHub kwin5786/repmate, NOT iLenSys).
Status: App is LIVE with 5 real members logging daily. Item 37 (admin search/sort/serial numbers) shipped at 035d6e3. Item 42 is next, as a STUDY not a build.

Standing rules apply (they live in memory — do not re-list them).

Opening ritual, in order:
1. Ask which machine I'm on today (ASUS or Mac — the repo exists only on ASUS at D:\Active Projects\Productive Apps\repmate).
2. Read this handoff fully.
3. Under "Here's where the project stands:", show the progress table and the status board from the handoff FIRST, before any prose.
4. Give me the git pull command for my machine (PowerShell block, output piped to clipboard). Wait for my confirmation that the pull is clean. Expect HEAD at or ahead of 035d6e3 on main.
5. This session is a STUDY. Bring a design document before any code: how calorie burn should be calculated for cardio vs weights vs reps, what extra fields each exercise type needs, the schema changes, what happens to the ~90 existing log rows, and the sources used. Cite real sources — do not invent MET values or formulas.
6. Then ONE recommendation, and WAIT for my explicit go.

Whenever you cite an item number, name what the item is alongside it.

Survey first: Claude Code reads index.html verbatim before proposing anything — never guess function names or structure.

Verification rule: every step gets a clickable link and literal click-by-click instructions. For destructive actions on shared data, confirm in BOTH the app and a database query — the two are separate claims. Row counts come from count(*), never from a dashboard estimate. The app has real users — treat every write path as touching someone else's data.
```
