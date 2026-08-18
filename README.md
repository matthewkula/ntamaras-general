<div align="center">

<img src="mark.png" alt="" width="88" height="88">

# Ntamara's General

**Record once. Command it forever.**

A Windows application that watches you do a task one time — in the browser *and* in
native applications — turns the recording into a script that uses your spreadsheet, and
repeats it until the work is done.

[**Download for Windows**](https://github.com/matthewkula/ntamaras-general/releases/latest) ·
[Read the guide](GUIDE.md) ·
[matthewntamara.com/tools/ntamaras-general](https://matthewntamara.com/tools/ntamaras-general/)

</div>

---

## What it does

You have four hundred rows in a spreadsheet and a form that needs filling in four hundred
times. You do it once, properly, while the General watches. Then it does the other three
hundred and ninety-nine.

It works on **web pages and on native Windows applications** — in the same recording. Copy
something out of a web page and paste it into an accounting program, four hundred times,
as one flow.

## Getting it

1. Download the zip from [Releases](https://github.com/matthewkula/ntamaras-general/releases/latest).
2. Unzip it anywhere — your Documents folder is fine.
3. Run `NtamarasGeneral.exe`.

Windows will show a blue **"Windows protected your PC"** box, because the app is not yet
signed with a paid certificate. Click **More info**, then **Run anyway**. You can verify
the download against the SHA256 checksum published with each release.

The first launch downloads the browser it drives — about 150 MB, once.

No installation, no administrator rights, no Python. Everything it writes lives in
`%LOCALAPPDATA%\NtamarasGeneral`, so removing it is one folder delete.

## Doing it once

1. **Record** — press Record, do the task properly, press Stop. Wait for the confirmation
   on screen before you stop; the app notices what you waited for and waits for it too.
2. **Check** — every step shows a picture of the control it touched, so you can see at a
   glance that step 4 really is the email box.
3. **Add your data** — a CSV, a JSON file, or paste straight out of Excel. Columns whose
   names match what you typed are mapped for you.
4. **Try one row** — then run the rest.

## When the application changes

Most tools of this kind break the moment a developer renames something. This one tries
three things before giving up:

- **Nine ways of finding everything.** Each control is recorded with several independent
  descriptions, so a renamed id costs nothing while the label survives.
- **It looks for the control that moved.** Free, offline, always on. It scores every
  candidate against what you recorded and writes the repair back, so the fix is permanent.
  It declines rather than guess between two similar candidates.
- **You can point at the right control.** One click, one step rewritten, nothing
  re-recorded.

## What it will not do

- **Record your passwords.** A password or one-time-code field produces a step with an
  empty value and a note telling you to set it yourself.
- **Wipe a document.** Clearing before typing is limited to controls that are clearly
  single-line fields.
- **Keep your clipboard.** It puts back whatever was there.
- **Loop forever.** Every stop rule has a hard limit behind it.
- **Send anything anywhere**, unless you switch on the optional vision feature and supply
  your own key.

## Requirements

| | |
|---|---|
| Windows 10 or 11 | for recording native applications |
| Any recent Windows | for browser-only work |
| About 400 MB of disk | app plus browser |
| Internet | first launch only, then only if your task needs it |

## Limits, stated plainly

- Desktop record and replay is verified on **Windows only**. A macOS version is written
  but has never been run on a Mac.
- The build is **unsigned**. The warning above is the consequence.
- Applications that expose no accessibility information at all fall back to image
  matching, then to coordinates — which is opt-in and always reported in the run log.

## Help

The full manual is [GUIDE.md](GUIDE.md), and it also ships inside the app under **Guide**,
so it works offline.

Something broken, or something missing?
[Open an issue](https://github.com/matthewkula/ntamaras-general/issues).

---

<div align="center">

Built by [Matthew Ntamara](https://matthewntamara.com) in Kampala, Uganda.

</div>
