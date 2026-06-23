# Tend — Back-Office Agent Suite (prototype)

A working front-end prototype for **Tend**: a back-office "operating layer" for an SMB
staffing firm. Five agents do the busywork; the owner (Brandy Mangum) reviews and approves.

This is a **design/UX prototype** — fully interactive, but state is in-memory and all data is
seeded/mock. It is meant as the spec + reference implementation for a production build.

---

## Run it
Open `Back office.dc.html` in a browser. No build step, no server.
Any email/password (or "Continue with Google") signs you in — auth is stubbed.

## Files
| File | What it is | Touch? |
|---|---|---|
| `Back office.dc.html` | The entire app — markup template + logic class | **Yes** — all app code lives here |
| `cha-data.js` | Seed data **and** initial UI state (`window.CHA_DATA`) | **Yes** — swap for real data source |
| `support.js` | Prototype runtime that renders the component | **No** — framework file, replace at prod |

### How `Back office.dc.html` is structured
It's a single component with two parts:
- **Template** — inline-styled markup with `{{ }}` value holes and `<sc-if>` / `<sc-for>` control flow.
- **Logic class** (`class Component`) — a React-class-like object. `renderVals()` returns everything
  the template binds to (one big object per active screen). State lives in `this.state`
  (seeded from `cha-data.js`); actions call `this.setState(...)`.

To find a screen's code, search for `RENDER: <name>` (e.g. `RENDER: Hiring agent`) in the logic,
and `SCREEN: <name>` / `OVERLAY: <name>` in the template.

---

## The five agents (+ screens)
- **Today** — ranked "what needs you" priorities across all agents, a "Done today" log, an
  overnight recap, deadlines/compliance, and a quote.
- **Client Health** — account health scoring, at-risk detection, drafted re-engagement notes.
- **Cash & Invoices** — cash log, AR/AP, a 2-month "what's due" calendar, and a **projected-cash
  "can you cover payroll?"** band.
- **Pipeline** — leads from first touch to signed; stage picker, proposal/signed forms, and a
  timestamped **journey** per lead.
- **Implementation** — onboarding tracker, CSM book, capacity bars, onboarding bottlenecks.
- **Hiring** — open roles, candidate pipeline (applied→hired), star ratings, **offer form +
  letter upload**, and a timestamped journey per candidate.
- **Team** — roster grouped by department, **editable** profiles (role/dept/manager/comp), employment
  status (active / on leave / departed → headcount & cost react), onboarding checklists, **people budget**.
- **Leadership** — revenue by segment, per-agent impact, book concentration, renewals.
- **Build Plan** — in-app explainer: what's built, what's mock, what's needed to go live, cost/savings.

---

## What's real vs. mock (engineer notes)
**Real (works in-prototype):** all navigation, every form (add team member, post role, add candidate,
offer, proposal, signed), candidate/lead/employee status changes with timestamped history, health
scoring math, cash projection math, CSV export, KPI/budget rollups. All client-side, no persistence.

**Mock / stubbed — needs wiring for production:**
1. **Persistence** — state resets on reload. Needs a real DB + API (replace `cha-data.js` reads and
   the `setState` mutations with server calls).
2. **Auth & roles** — sign-in is a stub. Add real auth + SSO + permissions.
3. **Notifications / Slack** — "drafted" actions and the overnight recap are simulated. Wire to
   Slack + email send (the human approves; nothing should auto-send without confirmation).
4. **Live data sync** — accounts, invoices, candidates, payroll are seeded. Connect to the CRM,
   accounting (e.g. QuickBooks), ATS, and inbox.
5. **File uploads** — the offer-letter upload captures the filename only; store the actual file.
6. **AI drafting** — re-engagement notes / job posts are templated strings; swap for a real model call.

See the in-app **Build Plan** screen for the same breakdown in product language, with a tool-replacement
and cost-savings estimate.

---

## Production rebuild guidance
The prototype's component/`renderVals` split maps cleanly to React components + hooks. Recommended:
keep the screen structure and data shapes (they're the product spec), replace `support.js` with your
framework, and replace `cha-data.js` with typed API models. The inline styles can move to your styling
system; the palette is a warm cream/sage set (greens `#5E8A6A` / `#2E4A38`, cream `#FBF7EF` / `#F5F0E6`,
amber `#C49A57`, text `#3A372E`).
