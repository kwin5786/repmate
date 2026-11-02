# Handoff — RepMate — Session 14 (2026-10-15)

**Handoff created by:** Karthik + Claude
**Project:** RepMate (gym tracker PWA — `kwin5786/repmate`, personal GitHub, NOT the iLenSys org)
**Track:** Single track — no parallel tracks.
**Branch:** `main`
**Code position:** `c31986d` — two commits this session. Pushes verified: `035d6e3..4d14a21`, then `4d14a21..c31986d`. Working tree clean (untracked docs only).

> Continues `Handoff_RepMate_Session13_2026-10-15.md`. Item 42 (workout detailing + calorie accuracy) is now **closed** — both halves shipped in one session.

---

## 1. Progress table

| # | Item | Phase / Module | Status | Last Touched | Notes |
|---|---|---|---|---|---|
| 16 | Progress photos | App v4 | ⏳ Planned | 2026-10-10 | Table empty |
| 17 | Exercise visuals — thumbnail + detail view | App v4 | 🟡 In Progress | 2026-10-15 | 6 of 85 wired. 44px thumbnails too small — consider 56px |
| 18 | Android APK via PWABuilder | Later | ⏳ Planned | 2026-08-20 | iOS native permanently out |
| 19 | Weight+Time combo type | Later | ⏸ On Hold | — | Workaround in place |
| 24 | Distance measure type (km) | App v3 | ⏳ Planned | 2026-10-15 | The `measure_type` CHECK constraint was never touched by item 42 — this stays a standalone job |
| 26 | Self-test blocks for the maths | Quality | ⏳ Planned | 2026-10-15 | Materially more valuable now the calorie maths has been rewritten twice |
| 27 | History pagination | Perf | ⏳ Planned | 2026-10-09 | Fetches every row ever logged |
| 28 | Parked ideas | Parked | ⏸ On Hold | 2026-10-09 | Gym layer, trainer role, invite codes |
| 29 | AI integration | — | ❌ Cancelled | 2026-10-15 | Re-confirmed this session — see §4 |
| 33 | Rollout message | Rollout | ✅ Completed | 2026-10-11 | 5 members onboarded in person |
| 34 | Seed the exercise library | Content | ✅ Completed | 2026-10-10 | 84 after the treadmill merge |
| 35 | Grouped + searchable exercise picker | App v4 | ✅ Completed | 2026-10-11 | commit c2a2bc4 |
| 36 | "Plan Today" empty state + warm-up toast | App v4 | ✅ Completed | 2026-10-11 | commit e611810 |
| 37 | Admin exercises — search, sort, serial numbers | Admin | ✅ Completed | 2026-10-15 | commit 035d6e3 |
| 38 | Avatar → Profile nav, drop Profile tab | App v4 | ⏳ Planned | 2026-10-15 | **Promoted — now blocks item 43.** The bottom bar is full at four slots |
| 39 | Timer for timed sets | App v4 | ✅ Completed | 2026-10-11 | commit e3e36fc |
| 40 | Member-changeable PIN | App v4 | ⏳ Planned | 2026-10-11 | All four new members still on `0000` |
| 41 | Member edit screen — rename, PIN reset, delete | Admin | ⏳ Planned | 2026-10-11 | Fully specified in Session 12 §5 |
| 42 | Workout detailing + calorie accuracy | App v4 | ✅ Completed | 2026-10-15 | **Both halves. commits `4d14a21` + `c31986d`** |
| 43 | Nutrition & calorie tracker module | App v5 | ⏳ Planned | 2026-10-15 | **Re-specified from scratch this session. See §5 — this is the next real build** |
| 44 | Library hygiene — duplicate merges | Content | 🟡 In Progress | 2026-10-15 | Treadmill Walk + Run merged. Three pairs still undecided |
| 45 | Treadmill set row cramped on mobile | App v4 | ⏳ Planned | 2026-10-15 | **New.** Six controls in one row; numbers clip on a phone |

Items 1–15, 21–23, 25, 30–32 remain ✅ Completed and unchanged.

**Legend:** ✅ Completed · 🟡 In Progress · ⏳ Planned · ⏸ On Hold · ❌ Cancelled

---

## 2. Status board

| Track | Branch | Last commit | Last session | Next step |
|---|---|---|---|---|
| RepMate (single) | `main` | `c31986d` | Session 14, 2026-10-15 | Item 38 (avatar → Profile nav) to free a bottom-bar slot, then item 43 (nutrition & calorie tracker) |

Shared files touched: no — single-owner project, single file (`index.html`).

---

## 3. State + what shipped this session

**Item 42a — calorie accuracy. ✅ `4d14a21`**

The study came first, as Session 13 demanded. Three real defects were found and fixed:

1. `SET_SECONDS` was 40 — forty seconds per set, rest counted as zero. Changed to **130** (~40s work + ~90s rest). This alone was the ~3× undercount.
2. `MET_KEYWORDS` matched substrings against the exercise name. `'crunch'.includes('run')` is **true**, so Crunches and Bicycle Crunch were being scored as running at 8.0 MET. Worse, "Stationary Cycling" does not contain the literal "cycle", so it never matched at all and fell through to 3.5 instead of 6.8. **Two of four cardio exercises wrong, and two non-cardio exercises wrong.** The whole keyword mechanism was deleted.
3. Replaced by a new column `exercises.default_met numeric null`, read by `metFor()` with `MET_BY_TYPE` as the fallback. Four rows tagged: Treadmill Walk 3.5, Treadmill Run 8.0, Stationary Cycling 6.8, EFX 5.0.

The survey caught a near-miss: all selects use explicit column lists, never `*`. `default_met` had to be added to the two joined selects feeding `logCalories()` (`loadTodayLogs`, `loadHistory`), the picker select, and `createExercise`'s insert-returning select. Without that the new branch would have been dead code and cardio would have been **worse** than before — the keyword overrides removed with nothing replacing them.

Verified live: Tue 13 Oct went from ~70 kcal to **355 kcal** across 12 exercises.

**Item 42b — treadmill speed and incline per set. ✅ `c31986d`**

Treadmill sets now carry `{secs, speed_kmh, incline_pct}`. New column `exercises.calorie_model text null`, set to `'treadmill'` on the treadmill rows — explicitly a column and not name-matching, since name-matching is what was just deleted.

New helpers `setHasPace()` and `paceMet()` implement the ACSM metabolic equations:

- Walking: `VO2 = (0.1 × S) + (1.8 × S × G) + 3.5`
- Running: `VO2 = (0.2 × S) + (0.9 × S × G) + 3.5`
- S in m/min, G as a fraction. **Switch at 8 km/h.** Between 6 and 8 km/h both equations drift; a fast walker at 7 km/h gets the walking equation and is slightly under-estimated. Accepted and documented in a code comment.

`logCalories()` now costs each time set on its own MET and sums, falling back to `default_met` for any set without a speed. Proven identical for speed-less sets by algebra: `Σ(met × secs) = met × Σsecs`, same single rounding. `formatSets()` renders `15m at 4km/h 2%`, omits a zero incline, and only applies the `x N` collapse when no set in the array has a speed. `bestPerDay` and `chartValueLabel` deliberately untouched — History charts still plot longest set in seconds.

Verified live at 68.5 kg: 15m @ 4km/h + 10m @ 6km/h 2% → **105 kcal**, matching hand calculation exactly.

**Treadmill merge (item 44 — library hygiene).**

Treadmill Run (1 log) merged into Treadmill Walk (4 logs), survivor renamed **Treadmill**. Verified: 85 total logs before and after, zero orphans, Elangkumaravelan's 13 Oct `[{"secs":300}]` present under the merged exercise. His run drops from ~46 to ~20 kcal because that row has no speed and the merged fallback is 3.5 — Karthik accepted this and will tell him.

**Item 43 — nutrition & calorie tracker: re-specified, AI dropped. See §5.**

---

## 4. Locked decisions made this session

| Decision | Rationale |
|---|---|
| Resistance work uses a flat MET over real elapsed time, **not** volume-load | Reverses Session 13's "volume-based model". The Compendium offers only four resistance values, none per-exercise; published volume equations are tied to specific protocols (3 sets to failure at 75% 1RM). Decisively: Karthik's own 0 kg bodyweight entries would score **zero** under any volume model, and the 110 kg calf raise would swamp a session |
| `SET_SECONDS = 130` (40s work + 90s rest) | The rest gap was the actual cause of the 3× undercount. No MET change could fix it |
| Keyword name-matching deleted permanently; MET comes from a column | `'crunch'.includes('run')` was live in production. Name-matching is unfixable in principle, not just in this instance |
| Cardio MET lives in `exercises.default_met`, cardio model in `exercises.calorie_model` | Two separate concerns: the fallback value, and which formula applies |
| ACSM walk/run switch at 8 km/h | Walking is validated ~3–6 km/h, running above ~8. Nobody walks above 8. The 6–8 gap is a known, documented under-estimate |
| Treadmill Walk + Treadmill Run merged into one exercise named **Treadmill** | The formula switches on speed, not on the exercise name, so two rows bought nothing. Cost: one member's historical run drops 56% |
| Historical rows are never recomputed — but they can't be protected either | Calories are **computed at render time**; there is no kcal column. Any formula change recomputes all history instantly. Hence the model must degrade gracefully on rows lacking new fields, which it does |
| Item 43 drops AI entirely — no photo, no natural language | There is no server. GitHub Pages serves one static file; any API key is publicly readable. This is item 29 (AI integration) re-confirmed, not re-litigated |
| Item 43's food library **grows as you use it**, per member — no bulk seeding | Reverses Session 13's "hand-seed ~150 foods". Karthik eats ~25 foods on rotation; typing each one once, the first time he eats it, builds the library in three weeks with no data-entry session and no licensing problem |
| Item 38 (avatar → Profile nav) is promoted ahead of item 43 | The bottom bar is full at four slots. Five is cramped on a phone. Item 38 frees one cleanly |
| Workout calories will **never** be added to the food allowance | The food target already includes usual training via its activity multiplier. Adding burn on top double-counts. "Eaten minus exercise" is never to be labelled a deficit — resting energy and daily movement dominate both |

---

## 5. Next session — focus, steps, and files

**Session 15 target: item 38 (avatar → Profile nav, drop Profile tab), then item 43 (nutrition & calorie tracker).**

### Item 38 — the warm-up, ~20 minutes

The **K** avatar at top right is currently decorative and not clickable. Make it open Profile; remove the Profile tab from the bottom bar, freeing the fourth slot for Food. Small, but it touches navigation all five members use daily — own survey, own verification.

### Item 43 — the full specification, as settled this session

Karthik asked that this be captured in full so it does not need re-explaining.

**Goal.** Quickly record what he eats and his activities, see approximate calories and protein, and share the daily record with ChatGPT for review. Manual logging only. No AI anywhere.

**Three screens, each reusing a pattern already in the app:**

| Screen | Reuses |
|---|---|
| Food tab — today's meals + totals block | Today tab's card list |
| Add food — pick from your list, set quantity | The exercise picker from item 35 |
| New food — name, unit, kcal, protein | The create-exercise sheet |

**Logging flow.** Tap +, search your own food list, tap the food, adjust quantity, done. Same muscle memory as logging a set. First time a food is eaten, it is typed once (name, portion, kcal, protein) and thereafter it is two taps. The "recent foods" list does the job of favourites and repeat-meal.

**Dashboard block at the top of the Food tab:**

```
Eaten        1,240 / 2,340 kcal     Remaining 1,100
Protein         78 / 130 g
Activity       ~310 kcal (estimate, not subtracted)
```

Activity sits **below a divider, greyed, visually outside the maths**. It is never subtracted from the allowance.

**Targets.** Both editable in Profile, never auto-prescribed. For reference, Mifflin-St Jeor for a 40-year-old male at 166 cm and 66.5 kg gives a resting rate of roughly 1,510 kcal/day; at a moderate activity factor (~1.55, training most days) that is around 2,340 kcal. **That number already contains football, gym and walking** — which is exactly why activity burn is displayed separately and never added.

**Schema — two new tables, plus two columns:**

- `foods` — `member_id, name, unit, kcal, protein_g` (per-member lists, deliberately unlike the shared exercise library; food is personal)
- `food_logs` — `member_id, log_date, meal (breakfast/lunch/snack/dinner), food_id, qty`
- `members` — `kcal_target`, `protein_target`

**Activities.** Pulled automatically from `workout_logs`. Manual activities (football, outdoor walk) are entered as `time`-type exercises in the existing library, so there is one source of truth and no double-counting. A post-lunch walk and a separate outdoor walk are two entries; that is correct, not a duplicate.

**Copy daily summary.** One button, plain text generated in the browser: foods, quantities, calories, protein, activities, burn estimates, weight. State plainly in the UI that **ChatGPT sees only what is pasted** — there is no connection between the two apps and nothing leaves the app on its own.

**Schedule.** Karthik finishes eating around 6 PM and trains later. An empty dinner slot is normal — no "incomplete day" nagging. This is a display rule, not a data rule.

**Explicitly OUT of v1** (each is fair, none is why the tracker would fail — it fails if logging a meal takes more than ten seconds): photos, natural-language parsing, planned-vs-eaten separation, weekly summary, favourites/repeat-meal as separate features, waist tracking.

**Open discrepancy to resolve first:** Karthik states his weight as **66.5 kg**, but the latest `body_weights` row is ~**68.5 kg** — every calorie figure verified this session checks out against 68.5. Reconcile before any target is calculated.

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
git commit -m "Session 15: short description"
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

Testing trigger: no suite yet. Item 26 (self-tests for the maths) is now overdue — the calorie calculation has been rewritten twice in one session and is verified only by hand arithmetic against screenshots.

---

## 6. New lessons

1. **A select with an explicit column list silently starves new code.** Adding `exercises.default_met` and writing `metFor()` to read it was not enough — all 24 selects in the file use explicit column lists, and the two feeding `logCalories()` did not include the new column. Shipping at that point would have made cardio **worse** than before, while looking fixed because weights moved. **Must not repeat:** when adding a column that code will read, grep every select that produces the object and verify the column is in each list, before testing.

2. **Substring matching on user-editable names is a latent production bug, always.** `'crunch'.includes('run')` had been scoring Crunches as running at 8.0 MET since the feature shipped, and nobody noticed because the number looked plausible. The same mechanism silently failed to match "Stationary Cycling" at all. **Must not repeat:** never derive behaviour from a name a user can type. Use a column.

3. **Verify a count baseline against reality, not against memory.** A "85 → 87" discrepancy was treated as a delete bug and chased for several turns; in fact seven real member workouts had been logged during the session and the two test entries had simply never been binned, because Claude had said "delete the test entries" as an aside without ever making it a step. **Must not repeat:** when a count moves unexpectedly on a live multi-user app, list the rows before diagnosing — and when an action is required, make it a numbered step, not a trailing remark.

4. **Checking the cost of a destructive action can reverse the recommendation.** The treadmill merge was called "cheap" before the query showing Treadmill Run held a real member's run with no speed recorded — making the merge a visible 56% drop on someone else's data. **Must not repeat:** run the cost query before recommending a merge, not after.

5. **A design handed down from a previous session still has to survive the sources.** Session 13 locked "a volume-based model for weights". Reading the literature showed the Compendium has no per-exercise resistance values and that volume-load equations are protocol-bound — and Karthik's own 0 kg entries would have scored zero. **Must not repeat:** a locked decision from a prior session is an input to the study, not a conclusion of it.

---

## 7. Open / parked backlog

- **Item 45 (treadmill row cramped on mobile) — new.** Six controls in one set row; `15` renders clipped on a phone. Preferred fix: drop the ⏱ timer button on treadmill rows (the console shows the time anyway) and widen the number boxes. Pairs naturally with item 17's 44px → 56px thumbnail question.
- **Item 44 (library hygiene) — three duplicate pairs still undecided**, all needing Karthik's judgement, all merges that preserve logs:
  - `Straight Bar Tricep` (2 logs) vs seeded `Cable Triceps Pushdown` (1 log)
  - `Pullover Chest` (2 logs, filed Chest) vs `Cable Lat pullover` (1 log, filed Back)
  - `Double D-Handle Lat Pulldown` (1 log) vs `Lat Pulldown` (4 logs)
- **Weights MET buckets — considered and deliberately skipped.** Every weights exercise is currently 3.5 MET, so a Romanian Deadlift and a Dumbbell Curl score identically. The honest improvement is three Admin-tagged buckets (compound 5.0 / isolation 3.0 / default 3.5) using the existing `default_met` column — no code at all, ~85 rows to tag. Judged not worth it: the fix moved the estimate from "wrong by 3×" to "right within ~30%", and 30% is near the honest ceiling for any non-calorimetry estimate.
- **Elangkumaravelan's 13 Oct treadmill run** drops ~46 → ~20 kcal after the merge, because that row carries no speed. Karthik is telling him.
- **Members not yet notified** of the calorie change at time of writing — their history roughly tripled. Draft message was prepared; Karthik is handling it.
- **Untracked in the repo:** `Discussiondocs/Handoff_RepMate_Session11…`, `…Session13…`, and `Logs/`. Recommendation was to commit the handoffs (they are the cold-restart anchor) and gitignore `Logs/` pending a check of its contents.
- **Read-only Supabase connection for Claude Code** — still worthwhile, still not set up. Would remove the copy-paste survey loop that ran twice this session.
- **Item 40 (member-changeable PIN):** all four members still share `0000`. Known, accepted gap.
- **Item 41 (member edit screen):** until it ships, removing a departing member is a dashboard delete that cascades their data away permanently.
- **Known soft spots, carried:** merge is sequential not atomic; same-day merges append both sets into one row; Friends `body_weights` query has no date floor; History fetches every row ever logged (item 27); `saveEntry` replaces the whole sets array per member+exercise+date.

---

## 8. Closing pause

The calorie number is now roughly right. It was wrong by about three times, and the cause was not the MET value everyone would have reached for first — it was that the app counted only the forty seconds you were lifting and nothing in between. Fixing that, plus deleting a keyword matcher that scored Crunches as running, plus giving the treadmill real speed and incline, took two commits and both were verified against hand arithmetic before they were pushed.

The thing that made this session work was doing the study first. The locked decision carried in from last time — volume-load for weights — did not survive contact with the sources, and saying so was more useful than implementing it.

What matters most next time: **item 43 (nutrition & calorie tracker) is specified in §5 in full, deliberately, so Karthik does not have to explain it again.** Read that section before proposing anything. The two things that will decide whether it gets used are that logging a meal takes under ten seconds, and that the food library builds itself from what he actually eats rather than from a data-entry session that never happens. Item 38 (avatar → Profile nav) ships first because the bottom bar has no room.

---

## Kickoff block for next session

```
Resuming a working session on RepMate.
Handoff attached: Handoff_RepMate_Session14_2026-10-15.md (continues Session 13's).
Branch: main (SOLO — single-owner project, personal GitHub kwin5786/repmate, NOT iLenSys).
Status: App is LIVE with 5 members logging daily. Item 42 (workout detailing + calorie accuracy) closed at c31986d. Next: item 38 (avatar → Profile nav) to free a bottom-bar slot, then item 43 (nutrition & calorie tracker).

Standing rules apply (they live in memory — do not re-list them).

Opening ritual, in order:
1. Ask which machine I'm on today (ASUS or Mac — the repo exists only on ASUS at D:\Active Projects\Productive Apps\repmate).
2. Read this handoff fully, including §5 — item 43 is specified there in full and must not be re-elicited from me.
3. Under "Here's where the project stands:", show the progress table and the status board from the handoff FIRST, before any prose.
4. Give me the git pull command for my machine (PowerShell block, output piped to clipboard). Wait for my confirmation that the pull is clean. Expect HEAD at or ahead of c31986d on main.
5. Survey first: Claude Code reads index.html verbatim before proposing anything — never guess function names, shapes or structure. Note that every select uses an explicit column list; a new column must be added to each select that produces the object.
6. Then ONE recommendation, and WAIT for my explicit go.

Whenever you cite an item number, name what the item is alongside it.

Verification rule: every step gets a clickable link and literal click-by-click instructions. For destructive actions on shared data, confirm in BOTH the app and a database query — the two are separate claims. Row counts come from count(*), never from a dashboard estimate. The app has real users — treat every write path as touching someone else's data.

This session: item 38 (avatar → Profile nav, drop Profile tab) first, then item 43 (nutrition & calorie tracker) per §5.
```
