# Changelog

## 1.3.2 — 22 August 2026

**The recording window comes to the front for real this time.** Version 1.2.3 said it made
the recorder's own browser window jump to the front and maximise itself. In the version you
actually download, that did nothing at all. The code that raises the window needs four
Windows modules, and it loads them inside the function rather than at the top of the file,
so the tool that packages the app never noticed one of them and left it out. The failure was
caught and thrown away in silence, so the app looked fine, every test passed, and the
feature had been dead in all three releases since.

The result was the exact problem 1.2.3 was written to prevent. The recording window opened
behind whatever you already had open, you carried on working in your normal browser, and
nothing was recorded, with nothing on screen to tell you.

The missing module is now packaged, and if raising the window ever fails again it says so
in the run log instead of going quiet.

## 1.3.1 — 21 August 2026

**The pop up warning now only fires on actual pop ups.** The check that flags a click as
having landed on something that was not the page treated anything floating above the page
as a pop up. A sticky header floats above the page too, and so does a navigation bar, so on
an ordinary news site about a quarter of every link on the page came back flagged, the main
menu included. A warning that cries wolf that often is worse than no warning at all. It now
separates the two by when they turn up: whatever is already there once the page settles is
furniture, and what arrives afterwards is a pop up. Cookie bars, newsletter boxes and full
screen adverts are still caught, including the ones with no useful name to go on.

**And it no longer mistakes itself for a pop up.** The recorder draws a small highlight
around each control as you click it. That highlight sits above the page like an overlay
does, so the recorder saw its own marker appear and flagged whatever you clicked next.
Any two clicks less than about a second apart were affected, on any page.

**Flows that run on their own can report again.** A flow pointed at a Zapier, Make or n8n
address only sent its summary when it was started from the window. Anything started from
the command line, which includes every scheduled run, finished without a word. Scheduled
runs are the main reason to want the summary in the first place. They now send it, and wait
long enough for it to arrive before the run closes down. The summary also carries a real
finishing time rather than a zero.

## 1.3.0 — 19 August 2026

**Recording now notices when something else steals your click.** A pop-up, a cookie
banner, or an ad landing where you meant to click used to become a step like any other,
indistinguishable until replay broke on it. Now that click is flagged the moment it
happens — in the recording review and in the editor — with a plain reason why, and a
one-tap way to remove it.

**And you can ask to be asked, on a page you already know is trouble.** A new
"Confirm each step before it's kept" option, off by default, pauses briefly after every
real step with a Remove button, right there in the browser you're recording in. Say
nothing and it's kept automatically after a few seconds — recording never stalls
waiting on you, it only slows down when you want it to.

## 1.2.3 — 19 August 2026

**The recording window now comes to you.** It used to be easy to miss which window was
actually being recorded, especially with other browser windows already open — clicking
around in the wrong one looked like nothing was happening, because nothing was. The
recorder's own window now jumps to the front and maximises itself the moment it opens,
so there is no mistaking it for anything else.

## 1.2.2 — 19 August 2026

**Repeating no longer needs a spreadsheet.** Right after recording, you now get a
straight choice: once per row of data, a fixed number of times, until something shows
up on screen, or until a step runs out of anywhere to go — the last one is built for
working through a feed or a list from a starting point to the end, since scrolling to
the next post failing on the last post is the natural stopping point. All four used to
be reachable only from deep in the editor; now they are the second question after a
recording, same as the data step used to be.

**Flows can now run themselves.** A new Automate tab lets Windows Task Scheduler run a
flow on its own — daily, weekly, hourly, or every few minutes — with no window open.

**And tell other tools when one finishes.** Also on the Automate tab: point a flow at a
Zapier, Make, or n8n webhook, and a summary posts there the moment the run ends.

## 1.2.1 — 18 August 2026

**Fixes a crash on startup.** Version 1.2.0 closed immediately on some machines with
`Unable to configure formatter 'default'` instead of opening. The app ships without a
console window, which left it with nowhere to write its startup messages, and the web
server refused to start rather than write nowhere. It now writes them to a file instead.

**There is now a log to look at when something goes wrong.** Anything the app would have
printed goes to `%LOCALAPPDATA%\NtamarasGeneral\logs\app.log`. If the app ever misbehaves,
that file is the first place to look, and the first thing to send me.

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
