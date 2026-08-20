# Handoff — RepMate — Session 3 (2026-08-19)

**Current focus:** Profile tab (onboarding stats age/height/weight/gender, BMI, body-weight chart) as the opener of Session 4, then Calorie burn, then PWA manifest.

**Track:** Single track — RepMate has no parallel tracks. (Two-Track Status Board not applicable to this project.)

---

## Progress Table

| # | Item | Phase / Module | Status | Last Touched | Notes |
|---|---|---|---|---|---|
| 1 | Stack decision (Supabase + GitHub Pages + UptimeRobot, ₹0) | Foundation | ✅ Completed | 2026-08-19 | Locked; ₹0 rule now HARD (see decisions) |
| 2 | Supabase project (Mumbai) + 5 tables + storage buckets | Database | ✅ Completed | 2026-08-19 | RLS on, permissive policies |
| 3 | GitHub repo `kwin5786/repmate` (public) + Pages hosting | Hosting | ✅ Completed | 2026-08-19 | Live at kwin5786.github.io/repmate |
| 4 | v1 app: login (name+PIN) + Today tab logging | App v1 | ✅ Completed | 2026-08-19 | All 9 verification tests passed |
| 5 | Logo (two-tone) live on login screen | Branding | ✅ Completed | 2026-08-19 | logo.png, commit f1dc950 |
| 6 | UptimeRobot keep-alive monitor | Ops | ✅ Completed | 2026-08-19 | Green/Up, 5-min interval |
| 7 | Timed sets (multi-set s/m, canonical secs, legacy compat) | App v2 | ✅ Completed | 2026-08-19 | All 10 checks passed, commit 357d682 |
| 8 | Date-picker button fix (latent v1 bug) | App v2 | ✅ Completed | 2026-08-19 | Same commit |
| 9 | appicon.png committed to repo | App v2 | ✅ Completed | 2026-08-19 | Commit bfce1c6 |
| 10 | History tab (day cards + calendar strip + SVG progress chart) | App v2 | ✅ Completed | 2026-08-19 | 12/12 checks passed incl. real phone, commit f91c537 |
| 11 | Profile tab (age/height/weight/gender, BMI, body weight chart) | App v3 | 🟡 In Progress | — | **Session 4 opener**; onboarding on first login |
| 12 | Calorie burn per workout (MET-based) | App v3 | ⏳ Planned | — | Needs Profile data (weight/age/gender) |
| 13 | PWA manifest + appicon (full-screen install, hide URL) | App v2 | ⏳ Planned | — | Must include `apple-touch-icon` for iPhone; lands before group rollout |
| 14 | Group tab (weekly counts, member view, compare) | App v3 | ⏳ Planned | — | Rollout gate |
| 15 | Admin screen (members, PIN reset, exercise edit/merge, images) | App v3 | ⏳ Planned | — | Rollout gate |
| 16 | Progress photos (front/side/back, private, compare dates) | App v4 | ⏳ Planned | — | Buckets already exist |
| 17 | Exercise images/GIFs on cards + set-entry screen | App v4 | ⏳ Planned | — | image_url/video_url columns ready |
| 18 | True APK via PWABuilder | Later | ❌ Cancelled | 2026-08-19 | ₹0 hard rule: no app stores ever; manifest covers install on both platforms |
| 19 | Weight+Time combo type (farmer's walk) | Later | ⏸ On Hold | — | Workaround: log as Time, weight in name |
| 20 | Dummy-data cleanup (18 Aug Plank test row) | Housekeeping | ⏳ Planned | 2026-08-19 | Row now holds `[{"mins": 2}]` from legacy test; delete when convenient |

**Status legend:** ✅ Completed · 🟡 In Progress · ⏳ Planned · ⏸ On Hold · ❌ Cancelled

---

## 1. Session focus

Session 3 set out to deliver the History tab (item 10) and delivered it completely: day cards, calendar strip, and hand-rolled SVG progress charts, verified 12/12 including a real-phone mobile test. The session followed Plan → Amend → Execute → Verify with one amendment round (restating the query + hardening the legacy-row test per Session 2's lesson). Two roadmap decisions were also made: app stores are permanently ruled out (item 18 cancelled under the ₹0 hard rule), and iPhone support was confirmed as covered by the PWA route with one `apple-touch-icon` addition noted for item 13.

## 2. Where we are exactly

- Date: 19 Aug 2026. Session 3 of the RepMate project (all three sessions on the same day).
- App is LIVE at `https://kwin5786.github.io/repmate/` with Today + History both fully working, tested on Karthik's phone.
- Repo: `kwin5786/repmate`, branch `main`, last commit `f91c537` ("Session 3: History tab - day cards, calendar strip, SVG progress charts"). This commit also added `Discussiondocs/Handoff_RepMate_Session2_2026-08-19.md` to the repo.
- Local path: `D:\Active Projects\Productive Apps\repmate` (ASUS only — Mac has no clone).
- Database: schema unchanged. 4 workout_logs rows. The 18 Aug Plank row now contains `[{"mins": 2}]` (edited during the legacy test) and is flagged for cleanup (item 20). Push Ups row edited to `12, 16, 10` during the edit-regression test — real data, keep.
- Immediate next move: Session 4 opens with the Profile tab.

## 3. What this session delivered

- **History tab (item 10), commit `f91c537`** — index.html only, 273 insertions / 6 deletions (plus the Session 2 handoff doc, 454 total insertions in the commit):
  - **Calendar strip:** sticky, last 14 days, dots on workout days, dotless days dimmed and dead to taps, tapping a dotted day smooth-scrolls to that day's card with a `.sel` highlight flash. Day cards carry `scroll-margin-top: 86px` so they land below the sticky strip.
  - **Day cards:** newest-first, rows reuse `formatSets` so display is character-identical to the Today tab; all user text through `escapeHtml`.
  - **Progress chart:** hand-rolled SVG (viewBox 320×200, no libraries). Best set per day — heaviest kg / longest secs (via `setSecs`) / most reps; same-day duplicates merged with max. 4 gridlines with unit-correct labels (`formatDuration` for time), index-spaced x-axis, first/middle/last date labels, last point value-labeled. Single-point mode: centered dot + value + "log more to see a trend" caption. Zero-point mode: honest "Not enough data to chart yet."
  - **Data:** one new select query filtered `.eq('member_id', session.member_id)`, ordered `log_date desc, id asc`. Charts computed client-side from the cached result. `historySeq` stale-response guard on tab flapping. History refetches on every tab entry, so Today-tab edits are never stale.
  - **States:** loading spinner, honest empty state, error state with message + error detail + working Retry.
  - **Read-only proof:** grep over the full diff found zero touches of `saveEntry`, `collectSets`, `confirmDelete`, or any Supabase write call.
- **Verification 12/12 passed:** day cards + strip render; dotless days dead; highlight flash; time chart (2 sessions, best-set math 900s > 30s); legacy `[{"mins": 2}]` row (Table Editor edit visually confirmed first) renders as 2m and plots as 120s; weight chart single-point mode (25kg); reps chart + best-set math (15 of 12/15/10); edit regression + freshness (16 propagated); error + Retry offline/online; member isolation via `member_id=eq.` in the request URL; real-phone mobile viewport; `node --check` clean.
- **Roadmap decisions:** item 18 cancelled (₹0 hard rule — Karthik: not even ₹1 on stores; Play $25 + Apple $99/yr rejected; PWA install covers both platforms). iPhone confirmed supported via Safari Add to Home Screen; `apple-touch-icon` line noted as a requirement for the item 13 manifest session.

## 4. Scope brief for next session

**Session 4 target: Profile tab (item 11), then Calorie burn (item 12) if time allows.**

Profile tab scope:
1. Onboarding on first login: capture age, height, weight, gender (one-time prompt for members missing profile data).
2. Profile view: stats display, BMI with a plain-language note, body-weight history chart (reuse the SVG chart pattern from History).
3. Body-weight logging into the existing `body_weights` table.

Why this is right: Profile feeds Calorie burn (MET formula needs weight/age/gender), and it's the locked roadmap order. Unlike History, this is **write-path work** — the first new save flow since timed sets — so the planning round matters more: privacy line must hold (body data private, never group-visible), and the onboarding flow needs a design decision (blocking vs skippable prompt).

Design judgment needed at session open (single recommendation posture): onboarding UX — block until filled vs skippable with a nag. Lean skippable (a gym app that interrogates you before letting you log a workout kills the habit loop); decide in the planning round.

## 5. Sources to read at session open

| File | Why |
|---|---|
| `Handoff_RepMate_Session3_2026-08-19.md` | This handoff |
| `index.html` in repo (Claude Code reads it) | The entire app; single source of truth for current behavior |

## 6. Files to attach at next chat open

This handoff file only. (Supabase keys live in Karthik's OneNote "RepMate Keys" note — never needed in chat; Claude Code reads them from index.html.)

## 7. Pre-chat shell commands

```powershell
cd "D:\Active Projects\Productive Apps\repmate"
$out = (git pull 2>&1) | Out-String -Width 4000
$out | Set-Clipboard
$out
```

## 8. Pre-derived per-task commands

Local test server for verification passes (leave this terminal alone while testing; Ctrl+C to stop):

```powershell
cd "D:\Active Projects\Productive Apps\repmate"
python -m http.server 8080
```

App at `http://127.0.0.1:8080`. All Profile tab work goes through Claude Code prompts (plan first, approve, execute).

## 9. Locked decisions (must hold)

| Decision | Rationale |
|---|---|
| ₹0 hard rule: no app stores, ever — item 18 cancelled | Karthik's explicit line: not even ₹1. PWA install covers iPhone (Safari) and Android; stores add fees, review delays, and stranger discoverability a private group app doesn't want |
| Manifest session must add `apple-touch-icon` (same appicon.png) | iPhone ignores parts of the manifest; without this tag iPhone members get a blank grey icon |
| Chart = hand-rolled SVG, no CDN chart libraries | ~80 lines, zero dependencies, instant load, matches single-file no-framework decision |
| Chart x-axis is index-spaced (by session), not calendar-time-spaced | Sparse/irregular gym sessions read better; dates appear as labels so nothing is lost |
| Y-axis: 20% padding around min/max, clamped at 0; flat-series guard | Flat progressions stay readable; nothing renders below the axis |
| Best set per day: heaviest kg / longest secs / most reps; same-day duplicate rows merged with max | One point per day keeps the trend honest |
| History refetches on every tab entry | Kills staleness after Today-tab edits; acceptable at current scale |
| Unbounded History fetch accepted for now | Flagged for future pagination; no mitigation by design at current scale |
| Rollout gate unchanged: manifest → Group → Admin, then share the link | Sharing earlier means URL-bar installs, no group features, manual member creation |
| All Session 1 + 2 locked decisions | Carried forward unchanged (free stack, single file, name+PIN, privacy line, canonical secs, lazy legacy migration, jsonb sets, personal GitHub, public repo, publishable key in index.html, link-only videos, private progress photos, roadmap order History → Profile → Calories → Manifest → Group → Admin) |

## 10. Mid-session hiccups (lessons — must not repeat)

1. **Claude Code's diff deletion count confused scope verification (21 → 6)** — resolved by checking git log: the prior session's work had been committed between sessions, so the uncommitted diff was purely the new feature. Must not repeat: before diff-based scope verification, confirm what's already committed so expected counts are grounded.
2. **Verification answer "just showed retry button only" was incomplete** — the screenshot then showed the full error state (message + detail + Retry) was present all along. Must not repeat: when a verification answer suggests a plan deviation, ask for the screenshot before logging a defect; conversely, verification questions should ask what text/elements are visible, not yes/no on the whole screen.
3. **Strip smooth-scroll couldn't be truly tested** — with only 2 day cards everything fits one screen, so no scroll occurs. Highlight flash verified instead. Not a defect; note: re-observe scroll behavior naturally once more days exist (no dedicated test needed).
4. **Commit swept in an unplanned file** — `git add .` picked up `Discussiondocs/Handoff_RepMate_Session2_2026-08-19.md` saved inside the repo folder. Harmless (documentation), but must not repeat unknowingly: before commit, glance at what `git add .` staged; keep non-app files out of the repo folder or accept them deliberately.

## 11. Approach posture for next session

- Plan → Amend → Execute → Verify for every Claude Code change; no punting decisions to runtime.
- One step at a time; wait for Karthik's result before the next instruction.
- Verification walkthroughs: one screen at a time, numbered steps, single yes/no question, screenshot each. When an answer is surprising, request the screenshot before logging pass/fail.
- Claude Code prompts = plain copyable text. Terminal commands = ```powershell colored blocks; verification commands pipe to clipboard with `2>&1` inside: `$out = (cmd 2>&1) | Out-String -Width 4000; $out | Set-Clipboard`.
- Single recommendation with reasoning, not option menus, when judgment is needed.
- Profile is **write-path work**: the planning round must fully resolve the save flow, table usage (`members` fields vs `body_weights` rows), and the privacy line (body data never group-visible) before execute.
- Verify against the live database (Table Editor) after data-writing changes; visually confirm jsonb/cell edits before testing against them.
- Ship verified before improving: finish + verify each feature before starting the next.
- Do not touch Supabase policies/tables unless the session plan explicitly includes it.
- Diagnose before fixing: root cause first.

## 12. Remaining work after this session

- Session 4: Profile tab (item 11) — onboarding, stats, BMI, body-weight chart. Then Calorie burn (item 12) — MET table per exercise/measure-type; duration for weight/reps exercises needs a design decision (timer vs ~30–45s/set estimate).
- Session 5: PWA manifest + appicon + `apple-touch-icon` (item 13), then Group tab (item 14) → Admin screen (item 15).
- Session 6+: progress photos (item 16), exercise images/GIFs (item 17).
- Housekeeping: delete the 18 Aug Plank dummy row (item 20) whenever convenient — Supabase Table Editor, workout_logs, log_date 2026-08-18.
- On hold: weight+time combo type (item 19). Cancelled: app-store APK (item 18).
- Rollout: after manifest + Group + Admin exist, add real members via Admin and share the install link on WhatsApp/Teams. iPhone members: "open in Safari → Share → Add to Home Screen" goes in the rollout message.

## 13. Closing pause

Three sessions, one day, and RepMate now does the two things a tracker must do: capture effort and show progress. The History chart turned the app from a logbook into a motivation engine, and it shipped with the same 100% verification discipline as everything before it — including a real-phone test. The most important thing for Session 4: Profile reopens the write path for the first time since timed sets, so the planning round carries the risk, not the coding. Resolve the onboarding UX and the privacy line completely before execute, keep the body-weight chart boring (the SVG pattern already exists — reuse it), and remember the app's real test suite is still Karthik logging real workouts every day.

---

## Kickoff block for next session

Copy the block below and paste it as your first message in the next chat. Attach this handoff file along with the paste.

```
═══ KICKOFF BLOCK — paste this at the start of your next chat ═══

I am resuming a working session. The handoff file is attached/uploaded.

Handoff file: Handoff_RepMate_Session3_2026-08-19.md
Project: RepMate (personal gym tracker — kwin5786/repmate, NOT an iLenSys project)
Current focus: Profile tab (onboarding age/height/weight/gender, BMI, body-weight chart), then Calorie burn, then PWA manifest

Before any work begins, please run the opening ritual in this exact order:

1. Ask: "Which machine are you on today — ASUS or Mac?" Wait for the answer. (Repo currently exists only on ASUS at D:\Active Projects\Productive Apps\repmate; if Mac, it needs a fresh clone first.)
2. Read the handoff file fully.
3. Display the progress table from the handoff first, before any prose. Add the line "Here's where the project stands:" above the table.
4. Give me the git pull command for my machine (PowerShell block, output piped to clipboard with 2>&1) and wait for my confirmation that the pull is clean.
5. Write a short paragraph (3-5 sentences) summarising current state, anchored to the Profile tab as current focus.
6. Open the Profile tab design discussion with a single recommendation (per handoff section 4: onboarding UX decision — blocking vs skippable prompt), then ask: "Ready to plan the Profile tab? Or want to adjust first?" Wait for my explicit confirmation before any Claude Code prompt is written.

Formatting rules for this project: Claude Code prompts as plain copyable text blocks; terminal commands as colored powershell blocks; verification commands pipe output to clipboard with ($out = (cmd 2>&1) | Out-String -Width 4000; $out | Set-Clipboard). Verification walkthroughs: one screen at a time, one question at a time.

Profile is write-path work: the privacy line (body data never group-visible) and the save flow must be fully resolved in the planning round before any execute approval.

Do not skip any step. Do not start work until I confirm in the final step.

═══ END KICKOFF BLOCK ═══
```
