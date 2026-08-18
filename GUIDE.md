# Ntamara's General — the guide

**Record once. Command it forever.**

This is the whole manual. It takes about ten minutes to read, and you only need the first
two sections to get work done today.

---

## 1. Installing it

### The easy way

1. Run **NtamarasGeneral-Setup.exe**.
2. Windows may show a blue "Windows protected your PC" box. That appears for any
   application without a paid certificate. Click **More info**, then **Run anyway**.
3. Pick a folder, or accept the default, and finish the wizard.
4. Open **Ntamara's General** from the Start menu.

The first launch downloads the browser it drives — about 150 MB, once. You need internet
for that first launch only.

### If you were given a folder instead of an installer

Unzip it anywhere and run `NtamarasGeneral.exe` inside.

### What it puts on your machine

Everything it writes lives in one folder:

```
C:\Users\<you>\AppData\Local\NtamarasGeneral
```

Your recordings, your spreadsheets, run reports, failure screenshots, and the browser
profile. To remove the app completely, uninstall it and delete that folder.

### What it needs

| | |
|---|---|
| Windows 10 or 11 | for recording native applications |
| Any Windows, macOS or Linux | for browser-only work |
| About 400 MB of disk | app plus browser |
| Internet | first launch only, then only if your task needs it |

---

## 2. Your first automation, in five minutes

We will fill in a form five times without touching the keyboard. Nothing real is at
risk — the app ships with its own practice form.

### Step 1 — Record

1. Open the app and click **Record**.
2. Name it something like `Practice run`.
3. Leave **Browser** ticked, and in **Start at** paste:
   `http://127.0.0.1:8777/demo`
4. Click **Start recording**. A browser opens with a red **RECORDING** badge.
5. Fill the form in properly, once: type a first name, last name, email, choose a
   district, type an amount, click **Save lead**.
6. Wait until you see the green "Saved lead for…" message. **This matters** — the app is
   watching for the thing you wait for, so replay knows when the work is finished.
7. Return to Ntamara's General and click **Stop and save**.

You now have a flow. Every box you typed into has already become a variable.

When you stop, the app asks you three questions in order. That is the whole product.

### Step 2 — Check the recording

Every step shows **a picture of the control it touched**, so you can see at a glance that
step 4 really is the email box. If something is wrong, discard and record again.

### Step 3 — Add your data

Click **Upload a CSV or JSON file** and pick `examples\leads.csv` from the installation
folder, or **Paste from a spreadsheet** and paste straight out of Excel.

Columns whose names match what you typed are mapped for you. Change anything that looks
wrong — the table shows what you typed, where it now comes from, and which box it goes
into.

### Step 4 — Try one row

Click **Run the first row**. Nothing else runs. If that row lands correctly, click
**Run the whole dataset**.

Trying one row first is the single most useful habit with this tool: if something is
wrong, you have changed one record instead of four hundred.

Everything below is refinement.

---

## 3. Recording well

The recording is the part that decides whether the automation is reliable, so it is worth
a few habits.

**Do the task properly, once.** Do not correct yourself mid-recording. If you fumble,
stop, discard, and start again — it takes fifteen seconds and saves you editing later.

**Wait for the result before you stop.** After your final click, wait until the screen
confirms it worked. The app records that confirmation as a checkpoint and uses it on
every replay.

**Do not record your password.** You do not have to avoid the field — the app already
refuses to store what you type into a password box, and leaves you a step with an empty
value to fill in yourself. Same for one-time codes.

**Recording native applications:** tick **Desktop applications** too. Then:

- **Never point it at an application holding work you care about** while you are
  learning. Use the practice app: `python -m general.ui.desktop_demo`.
- Keep the target window in front. Synthetic clicks go where the pixels are, so if
  another window is covering yours, the automation will act on that one instead.
- Click things properly — a real click on the real control, not near it.

**Recording both at once** is supported and is the point of the tool: copy something out
of a web page and paste it into an accounting program in a single flow.

---

## 4. Making it use your data

### Variables

Everything you typed while recording is a variable, with what you typed as its default.
So a flow with no spreadsheet attached replays exactly what you recorded. Attaching data
only adds to it — nothing breaks in between.

In the **Variables & data** tab each variable has:

| Column | What it does |
|---|---|
| **Variable** | The name, used as `{{name}}` in steps |
| **Dataset column** | Which spreadsheet column feeds it. Blank means use the default |
| **Default** | What you typed while recording |
| **Where** | Which control it types into |

### Data files

- **CSV** — first row is the header. Semicolons and tabs are detected automatically.
- **JSON** — an array of objects, or `{"rows": [...]}`.
- **Paste** — paste straight from Excel or Sheets.

### Using a variable somewhere else

Double-click any step to edit it. Type `{{column_name}}` anywhere in the value, or click
one of the variable buttons to insert it. A few extras are always available:

| Token | Gives you |
|---|---|
| `{{__index}}` | Which row you are on, from 1 |
| `{{__date}}` | Today, as 2026-08-18 |
| `{{__time}}` | The time now |
| `{{__uuid}}` | A unique reference |

You can also shape a value: `{{first_name\|upper}}`, `{{phone\|digits}}`,
`{{name\|trim}}`, `{{title\|slug}}`.

---

## 5. Controlling the run

### How many times — the Loop & safety tab

| Setting | Use it when |
|---|---|
| **Once per dataset row** | The normal case. One pass per spreadsheet row |
| **A fixed number of times** | Repeat the same thing N times |
| **Until a condition on screen** | Keep going until some text appears — "No more records" |
| **Just once** | A single pass, ignoring data |

**Safety limit** is a hard stop that always applies. Leave it on.

**Keep going after a failed row** decides whether one bad row ends the run or is logged
and skipped. Skipping is usually right for data entry.

### Speed

**As fast as the app allows** removes your recorded pauses. Start here and only slow it
down if the target application cannot keep up. The app never uses fixed sleeps of its
own — it waits for the screen, not the clock.

### Running several rows at once

Off unless you switch it on. When a flow has a few rows, the app measures this machine —
cores, free memory — and offers a number, showing its reasoning. You accept or decline.

Three things worth knowing:

- **Desktop flows always refuse.** There is one mouse pointer and one keyboard focus. Two
  rows at once would fight and both would fail.
- **Workers share the sign-in you established while recording**, but not anything written
  afterwards. Each worker is an isolated browser, so if the site you are automating keeps
  data *in the browser* rather than on a server, parallel workers will not see each
  other's work. Most real systems save to a server, where this does not apply.
- **The system you are automating has to cope too.** Six simultaneous sessions can look
  like an attack to a CRM. Start at two or three.

### Dry run

**Dry run** finds every control and clicks nothing. Use it after a website changes, to
check the flow still fits, without writing any data.

### When a step fails

Double-click a step and set **If this step fails**:

- **Give up on this row, try the next** — the default, right for data entry
- **Stop the whole run** — for anything financial or irreversible
- **Log it and carry on** — for optional steps like dismissing a cookie banner

---

## 6. When the target application changes

This is where most automation tools break and need re-recording. This one tries three
things before giving up.

**1. It kept several ways to find everything.** Each control was recorded with up to nine
independent descriptions. If the developer renamed an id, the label or role usually still
matches and the run continues. The log shows `(healed)` when this happens.

**2. It looks for the control that moved.** If none of the recorded descriptions match,
it scans the page for whatever best resembles what you recorded — same kind of control,
similar wording, same position in the form. If it finds a clear winner it uses it, writes
the new description into your flow, and carries on. The log says `heal:similarity`.

It deliberately gives up rather than guess between two similar candidates. Clicking the
wrong row is worse than stopping.

**3. Optionally, it can look at the screen.** If you switch on vision healing in
**Settings** and provide an Anthropic API key, a failed lookup sends one screenshot and
asks where the control went. The answer is converted back into a durable description and
saved, so you pay for it once, not once per row. There is a per-run limit on how many
times this can happen.

Vision healing is off until you turn it on. Everything else works without a key.

---

## 7. Settings

| Setting | Meaning |
|---|---|
| **Find moved controls** | The free, offline healing above. Leave on |
| **Use a vision model** | The paid fallback. Needs a key |
| **API key** | Stored on your machine, sent only to Anthropic when healing |
| **Model calls per run** | Hard ceiling, so a broken flow cannot run up a bill |
| **Match confidence / margin** | Raise to make healing more cautious, lower to make it bolder |
| **Save repairs to the flow** | Write healed descriptions back, so the fix is permanent |
| **Image matching** | About 90 MB, fetched only if you ask. Needed for controls with no name at all — rare |
| **Check for updates** | Off unless you give it a URL |

---

## 8. Running without the window

Useful for scheduled work.

```
NtamarasGeneral.exe list
NtamarasGeneral.exe run <flow_id> --dataset C:\work\leads.csv
NtamarasGeneral.exe run <flow_id> --headless
NtamarasGeneral.exe run <flow_id> --dry-run
```

It exits with a non-zero code if any row failed, so Windows Task Scheduler will show the
job as failed. To run every weekday at 7am, create a Basic Task pointing at the exe with
those arguments.

---

## 9. When something goes wrong

**"Desktop engine" is red in the sidebar.** Desktop automation is Windows-only. Browser
flows still work.

**The run says everything succeeded, but nothing was saved.** Almost always a missing
checkpoint. Re-record, and this time wait for the confirmation message before stopping.
You can also add one by hand: **Add pause** on the Steps tab.

**A step fails with "No element matched".** You have three options, in order of effort:

1. **Point it at the right control.** Click the ⌖ button on that step, or the *point it at
   the right control* link in the run log. The app opens the page and outlines whatever
   you hover over; click the correct control and that one step is rewritten. **Your click
   is not passed on to the page**, so pointing at a Delete button deletes nothing.
2. **Look at what happened.** Open the run in **History** and click the screenshot on the
   failed step. Usually a login page or a cookie banner appeared unexpectedly.
3. **Re-record**, if the task itself has changed rather than one control.

**I cannot tell what a step does.** Every step shows a picture of the control it touches,
taken when you recorded it. If a step has no picture, it does not point at a control —
navigation, pauses and scrolls have nothing to photograph.

**It typed into the wrong window.** The target window was not in front. Add a **focus
window** step, or arrange for the application to be open before the run starts.

**The browser asks me to log in every time.** It should not — the recording browser keeps
a profile. If it does, record a flow that logs in once, run it, then run your real flow.

**It is too fast for the application.** Set Speed to **Human pace**, or add a pause on
the step before the one that fails.

**I need to change one value everywhere.** Edit the variable's **Default** in Variables &
data. Every step using it follows.

---

## 10. Habits worth having

- **Dry run first** whenever the target has changed.
- **Test on three rows** before pointing it at three thousand. Set Loop to a fixed count
  of 3.
- **Keep the first row harmless** in any spreadsheet you are unsure of.
- **Export flows you rely on** (Export button) and keep the file somewhere safe. It is
  plain JSON and small.
- **Read the log after the first real run.** `(healed)` lines are telling you the target
  has drifted and the flow is one more change away from needing attention.

---

## 11. What it will not do

- Record what you type into password or one-time-code fields.
- Select-all-and-replace inside a document editor. It appends instead. Form fields are
  cleared normally.
- Keep your clipboard. It puts back whatever was there before it needed it.
- Loop forever. Every stop rule has a hard limit behind it.
- Send anything anywhere, unless you switch on vision healing and give it a key.
