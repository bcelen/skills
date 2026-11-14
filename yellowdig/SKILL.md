---
name: yellowdig
description: Build an on-demand HTML triage dashboard for a Yellowdig course discussion: pull posts via the data API and flag what the instructor needs to answer, each with a draft reply. Use when an instructor wants a Yellowdig report or to catch up on their community.
---

# Yellowdig triage

Produce an on-demand triage of a Yellowdig discussion community for its instructor: read the posts through Yellowdig's data API, decide what actually needs the instructor's attention, and deliver a single self-contained HTML dashboard where each flagged item links to the post and carries a copy-ready draft reply. This skill is agent-agnostic. It names no host-specific tools; use whatever your environment gives you for web requests, files, and delivery.

**Read-only, always.** Never post, comment, react, edit, or change anything in Yellowdig. You only read and report.

---

## 1. Collect inputs (interview the instructor)

State is kept per community in a small state file keyed by the community UUID (see §9 for the schema and where to put it). At the start of a run, look for that state.

**First run for a community (no state found)** — ask the instructor for:
- **Community URL** — the address of their open community tab. It contains the network name and community UUID, e.g. `https://yellowdig-<region>.app/n/<network>/community/<UUID>?...`. If they only know the subject name, you can list the network's communities (§3) and match the title with them.
- **API key** — generated in Yellowdig under User Options → Account Settings → API Keys → Generate Key (needs owner or facilitator role). **Secret. Ask for it every run, never store it in the state file or anywhere shared.**
- **Timezone** — for displaying post times (e.g. `Australia/Melbourne`).
- **Voice profile** — ask whether they have a writing-voice profile or skill you should draft replies in, and if so, to name it. Store only the name/reference in state, never its contents. If they have none, you will use a neutral lecturer tone.
- **Start point** — a baseline covers recent activity. Offer "the last 7 days" as the default, or let them name a date. You will also scan the whole cohort once for older still-unanswered mentions and blockers.

**Return run (state found)** — do not re-ask the settings. Confirm them in one line, ask only for the **API key** (it is never stored and it expires), and **propose the stored last-report end date as the new start date**, letting the instructor adjust it. No gap, no overlap.

**Every run, also ask once:** "Want brief one-to-two-sentence replies prepped for the newest 50 unanswered posts in the all-posts table?" (default no). If yes, generate them in §6 and show them in the table (§7).

If the session is unattended and no one can answer, use the stored settings, start from the stored cutoff, assume the brief-reply feature is off, and state these assumptions at the top of your output.

---

## 2. Access and auth

- **Region/host.** Derive the data host from the app host in the community URL: AU app `yellowdig-au.app` → data API `data.yellowdig-au.app`; US app → `data.yellowdig.app`. **A key only works on its own region's host** (the wrong host returns `{"error":"invalid API key"}`).
- **Auth.** Pass the key as the HTTP header `apikey: <key>`.
- **Key expiry.** Keys rotate and expire. A `401 {"error":"invalid API key"}` on the correct host means the key is dead, not a host problem. Tell the instructor and ask them to Generate Key again. Do not loop.
- **The data host is often blocked from direct server-side fetches.** If your environment cannot reach it directly, issue the requests from a browser or page context you control whose origin is the data host (so the call is same-origin and the `apikey` header is honored). If your environment can reach the host directly or has it allowlisted, call it directly. Either way, never weaken TLS verification. Because this usually needs a live browser, the report is run on demand.

---

## 3. Pull the data

All endpoints take the `apikey` header. `{network}` and `{UUID}` come from the community URL.

- **Find communities (optional):** `GET /network/{network}/communities` → list of `{community/title, community/squuid, community/created-at}`. Use to resolve a subject name to a UUID, or to spot a new cohort.
- **Full event history (primary pull):** `GET /network/{network}/community/{UUID}/events?limit=250&start=<epoch_ms>&offset=0` then `offset=250, 500, …` until the response is `at-end` or a page returns fewer than 250. `start=0` (or a cohort-start epoch) returns everything in ascending time. Dedupe by `event/squuid`.
- **Lifetime counts (for KPI tiles):** `GET /network/{network}/community/{UUID}/stats` → `member-count` and `event-counts` (post/comment/reaction/badge).
- **Newest events (sanity check only):** `.../latest-events` and `.../earliest-events` can **cap well below the `limit`** on busy communities, so never trust them as the full pull. Use them only to confirm the newest timestamp.

**Keep** `event/type` in {post, comment, reaction}; **drop** view, badge, points-granted (views are the bulk and are noise). Useful fields: `system/created-at` (epoch ms), `event/squuid`, `event/actor` (`user/firstname`, `user/lastname`, `user/username`, `user/squuid`, `user/community-role`), `post/title`, `post/body-text`. **Linkage:** comment → its post via `post/parent.event/squuid`; reaction → its post via `reaction/post.event/squuid`.

**Window.** Keep events with `system/created-at` greater than the start cutoff for the flagged triage. Separately scan the whole pull for older posts that are still unanswered and either @mention the instructor or report a blocker; comments often land on posts older than the window.

**Data-handling note.** If your tool output is size-limited or strips URLs and query-string characters, do not return large JSON through it. Accumulate results in your script or page, render only what you need into a text node or write it to a file, read that, and reconstruct permalinks yourself. **Permalink:** `https://yellowdig-<region>.app/n/{network}/community/{UUID}?postId=<event/squuid>`.

---

## 4. Identify people

From the actors in the pull: the **owner** is the instructor; **facilitators** are TAs or co-teachers. Treat owner + facilitators as **staff**. A post is **staff-answered** if any comment on it is by a staff member. Get the instructor's display name from the owner record and use it to detect **@mentions** (search post and comment bodies for the name tokens). Keep student emails out of everything you output.

---

## 5. Triage (lean)

Surface **only posts with no staff reply yet**. Read the actual post bodies to judge; do not rank by engagement counts alone. Be **lean**: flag only the clear must-reply items, not everything plausibly interesting. Three buckets, highest-value first:

1. **Tagged you / replies to your posts** — @mentions of the instructor and unanswered comments on posts the instructor authored. Often just thank-yous; if so, show a short positive empty-state line.
2. **Worth a reply** — genuine standouts to amplify, strong low-visibility posts (e.g. zero reactions) to surface, and any clear content slip worth a light correction or nuance.
3. **Blockers & time-sensitive** — access problems (broken links, paywalled readings, platform/login issues), logistics and group-formation friction, and anything tied to an imminent deadline. Most time-sensitive, so make these easy to see.

When a bucket is empty, say so with a short positive note rather than padding it.

---

## 6. Draft replies (instructor's voice)

Every flagged card carries a **copy-ready draft reply** the instructor can send as-is or edit.

- **Voice.** If the instructor named a writing-voice profile or skill (§1), draft in it. Otherwise use a clear, warm, slightly-informal lecturer tone. No em-dashes, no hype (do not tell students their work is "amazing" or their question is "great"); engage with the substance instead, warmth at the open and close.
- **Content/figure checks.** For "is this figure wrong in section X?" posts, keep the draft **method-based** and ask the student to confirm the specific number. The course figures are not in the pulled data, so never assert the numbers are right or wrong.
- **Brief-reply feature (if the instructor opted in):** also generate a one-to-two-sentence reply for each of the **newest 50 posts that have no staff reply yet**, to show inside the all-posts table.

---

## 7. Build the dashboard (format)

One self-contained HTML file (inline CSS and JS, no external calls). It must work at phone width, define colors as CSS variables, and support light and dark mode with an explicit `body` background.

**Header.** Eyebrow line "Yellowdig triage · baseline run" (use "rolling run" on return runs), the community title and term, then a subtitle naming the window (localized to the instructor's timezone) and the generated date.

**KPI tiles — exactly these six, in order. Do not include a Members tile:**
1. **Students** — enrolled students (`member-count` minus staff; if unavailable, distinct student authors).
2. **Active students** — distinct students who posted or commented in the window.
3. **Posts**
4. **Comments** (note how many are from staff)
5. **Reactions**
6. **Flagged for you** — count of flagged cards.

**A one-line summary callout** under the tiles: how well the community is covered (e.g. "X of Y posts already have a staff reply") and what the flagged list contains.

**Four separately collapsible panels, each a `<details>` in its own color tint** (tinted header bar plus a left accent border, all legible in light and dark):
- **Tagged you / replies to your posts** — tint A (e.g. violet).
- **Worth a reply** — tint B (e.g. teal/green).
- **Blockers & time-sensitive** — tint C (e.g. amber/orange).
- **All posts in the window** — tint D (e.g. blue/slate).

The three bucket panels open by default; the all-posts panel starts collapsed.

**Flagged cards** inside the bucket panels: post title linked to the permalink; a meta line (author · role · time · engagement); a one-line "why flagged"; a short quoted snippet; then the draft reply in a labeled box with a **Copy button**; and an "Open in Yellowdig" link.

**All-posts panel:** a searchable, sortable table of every post in the window — when, title (linked), author (staff styled distinctly), comment count, reaction count, and a staff-answered badge (answered vs open). Include a search box and an all / unanswered-only / staff-answered-only filter. If the instructor opted into brief replies, show the prepped one-to-two-sentence draft for each of the newest 50 unanswered posts in an expandable cell or extra column, each with its own Copy button.

**Footer:** note that it is a read-only triage, times are in the instructor's timezone, and drafts are suggestions that are not posted automatically.

---

## 8. Verify and deliver

- **Empty window:** if there are no new posts or comments since the start, do **not** build a dashboard. Report in chat what you found (e.g. only views/reactions, or nothing) and update the run log.
- **Verify before delivering:** if you can render HTML headlessly, load the file and check that the flagged-card count, table-row count, and copy-button count are right and that there are zero console errors. Otherwise re-read the generated HTML and sanity-check the counts.
- **Deliver** the HTML file through whatever your environment uses to hand files to the user. Optionally persist it as a dashboard/artifact if the host supports that, but do not block on it; the delivered file is the deliverable.

---

## 9. Save state and run log

After a successful run, write a per-community state file (somewhere durable the instructor's future sessions can read, e.g. a project doc or a known path; key it by the community UUID). Store everything except the API key:

```
{
  "community_uuid": "...",
  "network": "...",
  "region_host": "data.yellowdig-<region>.app",
  "app_host": "yellowdig-<region>.app",
  "community_title": "...",
  "instructor": {"name": "...", "squuid": "..."},
  "facilitators": [{"name": "...", "squuid": "..."}],
  "timezone": "Australia/Melbourne",
  "voice_profile": "<name/reference, or null>",
  "last_cutoff_iso": "<newest event timestamp from this run>",
  "run_log": [
    {"date": "...", "window_end": "...", "posts": 0, "flagged": 0, "note": "..."}
  ]
}
```

Set `last_cutoff_iso` to the **newest event timestamp from the pull** (trust the event data, not a local clock, which can lag). On the next run this is the proposed start date.

---

## Safety and privacy

- Read-only: never write to Yellowdig.
- The API key is secret: ask each run, never store or echo it.
- Keep student emails and other personal contact details out of the report.
- Leave sending the drafts to the instructor; nothing is posted automatically.
