# Changelog

## 1.2.0 — 18 August 2026

**Every step now shows a picture of the control it touches.** A step used to read
`click testid=save-lead`; now the step list shows the actual button, photographed as you
recorded it.

**When a step breaks, you point at the right control.** The app reopens the page,
outlines whatever you hover over, and captures your click — without passing that click on
to the page, so pointing at a Delete button deletes nothing. One step is rewritten; the
rest of the recording is untouched.

**Recording leads into a guided setup.** Three questions in the order they actually occur
to you: does this look right, what data should it repeat over, and shall we try one row.
*Run the first row* is now a button rather than a setting to discover.

**Several rows at once, if you want them.** Off by default. The app measures your machine
and shows its reasoning, then you accept or decline. Desktop flows always decline — there
is only one mouse pointer. Verified at 2× on three workers, with results identical to a
one-at-a-time run.

**The download is 111 MB smaller** (290 MB → 179 MB). Image matching is fetched on demand
from Settings, the same way the browser runtime is, because most flows never need it.

Also: a proper brand and app icon, a lint-clean codebase, and a fifth end-to-end test
suite covering parallel replay.

### Known limits

- Parallel workers run in isolated browsers. They share the sign-in you established while
  recording, but if a site keeps data *in the browser* rather than on a server, workers
  will not see each other's writes.
- The build is unsigned, so Windows shows a SmartScreen warning on first launch.
- Desktop record and replay is verified on Windows only.

---

## 1.1.0 — 17 August 2026

**Self-healing when a page changes.** If every recorded way of finding a control fails,
the app scans for whatever best matches what you recorded — same kind of control, similar
wording, same position in the form — and carries on. The repair is written back into the
flow, so it is paid for once rather than on every row. It declines rather than guess
between two similar candidates.

**An optional vision tier.** With an Anthropic key, a failed lookup can send one
screenshot and ask where the control went. Off unless you switch it on, and capped per
run so a broken flow cannot run up a bill.

**The guide ships inside the app**, under Guide in the sidebar.

**A Settings screen** for healing, keys and updates, and an optional update check.

**A macOS backend** written against the Accessibility API. Implemented, not yet run on a
Mac.

---

## 1.0.0 — 17 August 2026

First version. Record a task once in the browser or in native Windows applications,
turn what you typed into variables, and replay it over a spreadsheet.

- Nine independent ways of identifying every browser control, eight for every desktop one
- Smart waits throughout — no fixed sleeps
- Completion anchors: the app notices what you waited to see, and waits for it too
- Loops over CSV or JSON, or until something appears on screen
- Passwords are never recorded
- Two practice targets so nothing real is at risk while learning
