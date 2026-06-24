# Handoff: Tend — AI Back-Office Dashboard

## Overview
Tend is a back-office command center for owner-operators of B2B services firms. Its
premise: **five AI agents run the busywork; the owner just reviews & approves.** The
app surfaces what each agent has sensed and drafted overnight, and the owner approves,
edits, snoozes, or dismisses — nothing auto-sends. It covers Client Health, Pipeline,
Cash & Invoices, Implementation/CSM, and Hiring, plus back-office tools (Leadership,
Team, Notes & Tasks, Activity log, Templates, Build Plan).

## About the Design Files
The files in this bundle are **design references created in HTML** — a working,
high-fidelity prototype showing the intended look, copy, and behavior. They are **not
production code to ship directly.** The task is to **recreate these designs in the
target codebase's environment** (React/Next, Vue, etc.) using its established patterns,
component library, and state/data layer — or, if no codebase exists yet, to pick an
appropriate stack (e.g. React + a component lib) and implement there.

`Back office.dc.html` is authored in a small in-house template runtime ("DC"): inline-
styled HTML markup (the part between `<x-dc>…</x-dc>`) bound to a plain-JavaScript
`class Component` (state + handlers + a `renderVals()` that returns per-screen view
data). Treat the markup as the **visual spec** and the class as the **behavior spec**.
Re-implement both in idiomatic code for the target stack; do not port the DC runtime
(`support.js`).

## Fidelity
**High-fidelity (hifi).** Final colors, typography, spacing, copy, and interactions are
all present and intentional. Recreate the UI to match. Exact values live inline in
`Back office.dc.html`; the most-used tokens are listed under **Design Tokens** below.

## Layout Shell
- **Two-pane app**: a fixed **left sidebar** (248px) + a **main column** (flex, fills
  the rest). Whole app is `height:100vh; overflow:hidden`; only the main content area
  scrolls.
- **Sidebar** (`#EDE6D7`, 1px right border `#E0D8C6`), top → bottom:
  - **Brand**: 30px rounded-9px green (`#5E8A6A`) mark + "Tend" wordmark (17px/700).
  - **Today**: pinned nav item (dashboard).
  - **"Your agents"** section label (10px uppercase, `#A09781`) + a thin `#DFD6C4`
    divider line, then the 5 agents as a roster. Each agent item is a **two-line** nav
    button: row 1 = colored 30px icon + name (13px/600) + count badge; row 2 = a live
    status line (11px `#8A8472`) preceded by a small pulsing dot. This live roster is
    the core "agents are working" expression — keep it.
  - **"Back office"** section label + divider, then 6 single-line nav items (icon 26px
    + name 12.5px/600 + optional count).
  - **Account footer** (pinned bottom, top border): 34px avatar "BM" + "Brandy Mangum"
    / "Hartline" + a sign-out icon button.
  - Active nav item = white background + soft shadow `0 1px 3px rgba(60,55,46,.07)`;
    inactive = transparent.
- **Main column**: a **header** (title + "Live" pill + subtitle on the left, a
  "12 hours saved / this week" stat box on the right) over a scrolling content area
  (`padding: 24px 28px 40px`).
- **Responsive**: below 860px the sidebar goes off-canvas (`translateX(-100%)`) and is
  toggled by a hamburger (`.app-hamburger`) with a dim backdrop (`.app-backdrop`);
  `.app-root.nav-open` slides it in. The hours-saved box hides on small screens.

## Screens / Views
All screens render inside the main scroll area and are switched by
`this.state.active`. Each is a `data-screen-label` section. Names below are
(label — `active` id).

1. **Today — `today`**. The morning briefing. Green gradient greeting banner
   (`linear-gradient(135deg,#5E8A6A,#7BA17D)`); a 2-col row of **"What your agents
   saved you"** (4 stat cards in a 2×2: Hours saved, Revenue kept, Saves, Drafts
   ready) and **"Worked overnight"** (a dark-green `#2E4A38` panel of timestamped
   agent actions; the two columns are equal-height/stretch so neither leaves dead
   space); a **"Top priorities"** two-column grid of action cards (tag pill, title,
   one-line context, sub-note, dark CTA button, colored left border by source); then
   optional **Done today**, and a 2-col **Deadlines & compliance** / **Bills due this
   week**.

2. **Client Health — `health`** (default screen). Left: an "agent sensed → drafted →
   awaiting you" status strip; a 5-up KPI rollup (In view / At risk / Watch / Healthy /
   Upsell — each clickable to filter); a **Accounts** list (avatar, name, segment pill,
   health bar + score, annual value, last touch, renews; rows expand to show what the
   agent noticed + a drafted email with Approve & send / Snooze / Dismiss). Right rail:
   a **Renewals — next 90 days** card (purple `#7B6FA0` accent; company · date · risk ·
   amount, click opens the account) over a **What I'm watching** live-triggers card.

3. **Leadership Overview — `leadership`**. KPI roll; "Where the revenue is" (segment
   bars); "What your agents delivered" (2×2 impact tiles); a 2-col "Book concentration"
   / "Renewals — next 90 days".

4. **Pipeline — `followups`**. Lead board (kanban) / leads list toggle, pipeline
   stats, and a 3-column **Renewals** section (On track / Follow up / At risk buckets).

5. **Implementation & CSM Book — `schedule`**. Onboarding tracker (expandable
   milestone lists), CSM capacity, book split.

6. **Cash & Invoices — `cash`**. A **Cash log / Insights** tab toggle; a hero
   **"Cash agent"** dark-green panel answering "can I make payroll?" (sensed figures +
   a verdict + a drafted cash-forward action); a 6-up KPI grid (Cash on hand, in, out,
   Net, Monthly burn, Runway); a 3-month "What's due" calendar; the cash log ledger
   with add-transaction + Export CSV.

7. **Hiring — `hiring`** / **Team — `team`** (same branch). Roles, candidate pipeline,
   offers; team roster with profiles, comp, onboarding.

8. **Notes & Tasks — `notes`**. Worklist, "Up next today", a week summary, a month
   calendar, last-3-months renewals.

9. **Activity log — `log`**. Full timeline of everything you and the agents did
   (who = Agent vs. You, color-coded).

10. **Templates — `templates`**. Outreach template library by category, with a compose
    flow.

11. **Build Plan — `build`**. In-product "what's built vs. what's needed to go live"
    (and a feature status table: In-app / Mock data / Mock). **Read this screen** — it
    is the product's own go-live checklist.

**Overlays** (render above the shell, `position:fixed`): account-health drawer, lead
detail drawer, transaction drawer, team-member profile, employee modal, escalation
modal, and several "add" forms (customer, lead, bill, team, role, candidate, offer),
plus the **login** screen (split layout: form left, green marketing aside right;
`notSignedIn` gate, `z-index:120`).

## Interactions & Behavior
- **Navigation**: clicking a sidebar item sets `this.state.active`; the matching
  `data-screen-label` section shows. Agents and Today/back-office tools are all nav
  targets.
- **Human-in-the-loop**: every agent flag produces a *drafted* action the owner
  Approves / Edits / Snoozes / Dismisses. Nothing sends automatically. Approvals push
  an entry into the Activity log and a toast.
- **Filtering**: Client Health KPI cards filter the accounts list by segment.
- **Expand/collapse**: account rows, onboarding milestones, lead/transaction details.
- **Forms & drawers**: slide in from the right (`justify-content:flex-end`); modals
  center. Add-forms validate required fields and emit a toast on success.
- **Toasts**: transient confirmations (`notify(text, kind)`), color-coded by kind.
- **Live touches**: pulsing status dots on agents (`@keyframes pulsedot`), a `popin`
  entry animation on screens (`animation: popin .2s ease`).
- **Snooze**: opens a duration picker (3 days … 90 days).

## State Management
All state is a single in-memory object on the component (`this.state`, seeded from
`window.CHA_DATA`). Key slices:
- `signedIn`, `active` (current screen), `navOpen` (mobile drawer).
- Domain data: `items` (accounts), `leads`, `payroll`, `bills`, `cashTx`, `team`,
  `openRoles`, `templateCats`, `tasks`, `teamNotes`, `history` (activity), plus
  `EXTRAS` (per-account contract/segment metadata).
- UI/transient: `expanded`, `openAccount`, `openTxId`, `snoozeFor`, drawer/modal ids,
  and the various `*Form` objects for add flows.
- **State transitions** are plain methods on the class (e.g. `setStatus`, `signLead…`,
  `addTx`, `applySnoozeDays`, `submitLead/Cust/Bill/Team/Role/…`). `renderVals()`
  derives all view data from state per screen.
- **No persistence today** — state resets on reload (see Build Plan). Production needs
  a database and an API; replace `CHA_DATA` seeding and the in-class mutations with
  real fetches/mutations.

## Design Tokens
**Colors**
- Surfaces: app bg `#F5F0E6`; sidebar/header `#EDE6D7`; cards `#FBF7EF` / `#F7F2E8`;
  white `#FFFFFF`; login bg `#F0EADC`.
- Dark panels (agent heroes): `#2E4A38`, gradient to `#46624E` / `#3C5944` / `#2C4234`.
- Borders: `#E0D8C6`, `#E7DFCE`, `#E6DECC`, `#ECE3D1`, `#DFD6C4`, `#E4DCCA`.
- Text: `#2E2B24` (primary), `#3A372E`, `#46443C`, `#544F43`, `#6E6857`, `#857F6E`,
  muted `#A09781` / `#A39C88`, faint `#B7AE98`.
- Brand green: `#5E8A6A` (primary), `#7BA17D` (light), `#4F7560`; success chip bg
  `#E6EFE6` / text `#4F7560`.
- Gold/amber: `#C49A57`, `#C7A25A`, `#9A7634`; badge bg `#C7A25A`.
- Risk: at-risk/terracotta `#C17A60` / `#A65C42`; warn `#C49A57`; chip bg `#F3E4DC`.
- Per-agent accents (icons): Client Health green `#5E8A6A`, Pipeline blue `#6E8FBE`,
  Implementation purple `#9A7CB0`/`#7B6FA0`, Cash gold `#C49A57`, Hiring terracotta
  `#C77D54`. (Exact values per agent are in `agentDefs` in the class.)
**Typography**: Figtree (400/500/600/700), via Google Fonts. Page/screen titles
18–20px/600 (letter-spacing −.3px); section headers 14px/600; body 12.5–13px; labels
10–11px/600 uppercase (`letter-spacing ≈ .07em`); big stats 22–30px/700
(letter-spacing −.4 to −.6px); line-height ~1.4–1.5 for prose.
**Radius**: cards 13–16px; pills/buttons 10–14px; small icons 8–12px; count/status
badges 20px (fully rounded).
**Shadows**: resting `0 1px 2px rgba(60,55,46,.04)`; nav-active `0 1px 3px
rgba(60,55,46,.07)`; raised/active card `0 4px 14px rgba(60,55,46,.12)`; drawer
`0 18px 50px rgba(40,36,28,.3)`.
**Spacing**: page padding 24–28px; card padding 13–18px; common gaps 8 / 10 / 12 / 16 /
22px.
**Motion**: `popin .2s ease` (screen enter); `pulsedot 1.6s ease-in-out infinite`
(live dots); sidebar slide `transform .26s cubic-bezier(.4,0,.2,1)`.

## Assets
- **Fonts**: Figtree (Google Fonts) — already linked in the file's `<helmet>`.
- **Icons**: Unicode glyphs (e.g. ♥ ↗ ◷ $ ⊕ ✦ ☀ ⏻ ◆ ◑ ✓ ≡ ✉ ▤). Substitute your
  codebase's icon set (e.g. Lucide/Phosphor) to taste; shapes/semantics are what matter.
- **No raster images / logos** — the brand mark is a simple colored square + wordmark.

## Files
- `Back office.dc.html` — the full design (markup = visual spec; the `<script>` class =
  behavior/state spec). **Primary reference.**
- `cha-data.js` — seed/demo data (`window.CHA_DATA`, `window.CHA_EXTRAS`). The data
  model to mirror in real APIs.
- `support.js` — the DC runtime. **Do not port**; it only matters to run the prototype.
- `index.html` — the same app bundled into one self-contained file. Open it in a
  browser to see the design running (any input signs in). For reference only.

### Running the prototype
Open `index.html` directly in a browser, or serve this folder with any static server
and open `Back office.dc.html`. No build, no dependencies.
