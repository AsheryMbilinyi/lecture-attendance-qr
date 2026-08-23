# Lecture Attendance QR

A single-file, no-backend tool for taking classroom attendance with a QR code you project on screen. Unlike a static QR code (which students can screenshot and forward to friends who aren't in the room), this one **rotates to a new code every 15–60 seconds**, so a screenshot goes stale within seconds.

It doesn't require a server, an account, or an install. It's one HTML file you open in a browser and project.

**[Try it live](#) — or just open `index.html` in any browser.**

---

## How it works

1. You open `index.html` and start the session.
2. Every N seconds (you choose the interval), the page generates a random 6-character code and a fresh QR code that encodes it.
3. Students scan the QR code with their phone camera. If you've connected it to a Google Form (see below), the code is **pre-filled automatically** — students just add their name/ID and submit.
4. Every code generated is logged on screen with a timestamp, for the whole session.
5. After class, you cross-check submitted codes in your Form responses against the session log. Anything outside a valid time window is a stale/forwarded code and can be discarded.

This doesn't make cheating impossible (a determined student can still edit the pre-filled field before submitting), but it makes forwarded screenshots expire almost immediately, and it makes after-the-fact auditing trivial — which is a big step up from a single unchanging QR code.

---

## Quick start (no setup required)

1. Open `index.html` in a browser.
2. Click **Start lecture**.
3. Project the page. That's it — it'll just display the rotating code and QR for testing purposes.

This alone doesn't collect attendance anywhere; it just shows you the mechanism. To actually capture responses, connect it to a Google Form:

## Connecting to a Google Form

1. Create a Google Form with the fields you want, e.g.:
   - Name
   - Student ID
   - **Code** (short answer — this is the important one)
2. Open the form, click the **⋮** menu (top right) → **Get pre-filled link**.
3. Fill in sample values for Name/ID, and in the **Code** field type the literal word `CODE` (all caps, exactly that).
4. Click **Get link**, then **Copy link**.
5. Paste that link into the "Form link template" box in the tool.
6. Click **Start lecture**. The tool will swap `CODE` for the live token on every refresh automatically.

After class:

- Open your Form's response spreadsheet.
- Compare the submitted "Code" values against the tool's session log (visible in the sidebar, or copy it out as CSV with the **Copy as CSV** button).
- Any submitted code that isn't in the log, or whose timestamp is far outside the code's live window, is a stale or forwarded submission.

---

## Settings

| Setting | What it does |
|---|---|
| Course / section label | Just a label shown at the top of the display — no functional effect. |
| Code refresh rate | How often a new code (and QR) is generated. 15–20s works well for most lecture halls; go longer (30–60s) for bigger rooms where students are further from the screen. |
| Form link template | Your Google Form's pre-filled link, with `CODE` in place of the actual code value. Leave blank to just test the rotation without a real form. |

---

## Running multiple sections or sharing one hosted link

If several profs (or one prof teaching several sections) all use the same hosted page — e.g. one shared GitHub Pages URL — there's no server-side state to worry about, but you don't want to re-paste your Form link every time or risk projecting the wrong section's form.

Fix: after filling in your course label, interval, and Form link, click **Copy my setup link**. This bundles those three fields into the page URL itself, e.g.:

```
https://yourname.github.io/lecture-attendance-qr/?course=CSC445-A1&interval=20&form=https%3A%2F%2Fforms.gle%2Fxxx%3Fentry.123%3DCODE
```

Bookmark that link. It loads pre-filled every time, and it's entirely self-contained in the URL — nothing is stored on a server or shared between sessions. Each section gets its own bookmark, so there's no collision even if four different profs (or one prof with four sections) are all using the same hosted page.

---

## Limitations, honestly

- **Not tamper-proof.** A student could still hand-edit the pre-filled code field, or dictate the current code to a friend by voice/text in real time. This tool raises the bar significantly (screenshots go stale in seconds) but doesn't eliminate proxy attendance the way geofencing or device-based tools do.
- **No auto-sync to Canvas/Brightspace/etc.** Responses land in a Google Sheet; exporting into your LMS gradebook is a manual step.
- **Session state doesn't persist.** If you refresh the browser tab mid-lecture, the session log resets. Don't reload the page once you've started.
- For large lecture halls with a known cheating problem, consider a paid tool with geofencing/device fingerprinting instead (e.g. Attendzy, Squarecap) — this project optimizes for "free, simple, and good enough for most classes," not maximum fraud resistance.

---

## Sharing / hosting

This is a single static HTML file — no build step, no dependencies beyond a CDN-hosted QR library. A few ways to use it:

- **Just open the file locally** each lecture (works fully offline except for the one CDN script tag).
- **Host it for free via GitHub Pages**: enable Pages on this repo (Settings → Pages → deploy from `main` branch, root), and it'll be live at a shareable URL.
- **Fork it** to tweak colors, add your institution's branding, or change the default code length/interval.

---

## License

MIT — use it, modify it, share it with colleagues freely. See `LICENSE`.
