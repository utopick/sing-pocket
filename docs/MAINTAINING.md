# Maintaining SING.Pocket

A guide for whoever looks after this code next. It explains how the app is put together, in plain words, so you can
find your way before you change anything. It describes `main`, the installable web app (PWA).

## What it is
SING.Pocket is a vocal coach: warm-ups (and your own exercises), song practice with key and tempo change, lyrics, a pitch test, recorded takes, a range finder, autotune, a voice sampler and a plane game. It runs in any modern browser and installs as a web app (PWA).

## The one-file rule
The whole app is **one file: `index.html`** (plus `midi-device.js` for the MIDI panel, and `sw.js`, `manifest.webmanifest` and `icons/` that make it installable) (about 8,000 lines). There is no build step, no framework and no
dependencies to install, so anyone can open it, read it and change it.
- Inside, in this order:
  - a comment at the top that lists the layout of the file and every storage key (read it first)
  - `<style>`
  - the markup: one `<section>` per view
  - one big `<script>`
- To find something, search for its name. Views are sections with an id; buttons have ids; the code wires them with
  `$("#id").onclick = ...`.
- `$` is `document.querySelector` and `$$` is `querySelectorAll`. `toast("...")` shows a short message.
- The help texts live in the `HELP` array, in English (`en`) and Spanish (`es`). When you change what a button does,
  change its help in both.

## The views
`warm`, `song`, `range`, `at` (autotune), `vox` (sampler), `game` (plane), `prog` (history), `takes` (takes and files).
`showView` / `showTab` switches between them: it shows one section and asks that view to draw itself.

## The state: where things are kept
- `S`: what the user chose (voice type, exercise, tempo, key, options...). It is saved as `sing.state`.
- `EXS`: the warm-up exercises. The user's own exercises (`S.cex`) are added to it, so the warm-up and the plane
  treat them like the built-in ones.
- `VX`: the sampler's 16 pads and its recording. `W`: the warm-up while it runs. `P`: the song player.

Rule of thumb: change the state, then call the view's `...Face()` or `render...()` function to redraw it. Don't
change the screen by hand and hope the state follows.

## Sound
All sound is the Web Audio API: an `AudioContext` (`ctx`), made on the first tap because browsers only allow sound
after a gesture.
- The microphone feeds a pitch detector; the warm-up, the pitch test and the plane read it every frame.
- Songs are time-stretched with WSOLA (key and tempo change without the chipmunk effect).
- The sampler records through an AudioWorklet that stamps every block with the audio clock (as in TUNE's looper).
  Its effects are rendered ("baked") once, offline, so playing a pad costs nothing.

## Storage
- localStorage: `sing.state`, `sing.log` (history), theme, language, the MIDI map (`sing.midi.map`).
- IndexedDB `sing-pocket`: songs (with their audio), takes, and the sampler pads.
- The only network request is the lyrics search (lrclib.net), and only when FIND is tapped.

## MIDI (experimental, `midi` branch)
The same block is pasted in all three apps, starting at the comment "MIDI IN (experimental". Change it in one
app, then copy it to the other two so they stay identical.
- `midiConnect()` asks the browser for MIDI. Chrome, Edge and Firefox have it; Safari and iPhone don't.
- `midiMsg()` receives every message:
  - A control the user ASSIGNed runs its action (`MI.map`, `MI.actions()`).
  - Other notes go to `MI.on` / `MI.off`, which each app defines.
  - The sustain pedal (CC 64) delays `MI.off` until it comes up.
- ASSIGN (`midiAssign`) walks through `MI.actions()`, one press per action.
- TEST (`midiTest`) lists what a device sends and copies a report. Ask users to paste it in issues: it says exactly
  what their controller does.

## The exercise editor in two minutes
- `#exEd` is a small piano roll. `ED` holds what is on screen.
  - `edFromNd` / `edToNd` turn an exercise into grid cells and back.
  - An exercise is a list of `[degree, beats]` played on every key of the warm-up.
- `edRec` records from a MIDI keyboard, with a click.
- `exNote` plays a note with the piano, or with a sampler pad retuned to that pitch.

## The web app (PWA) and the Android leftovers
- `manifest.webmanifest`, `sw.js` and `icons/` make the page installable. The service worker keeps the app files for
  offline use and tries the network first, so an online visit gets the newest version. If you add a file the page needs
  at start (like `midi-device.js`), add it to the list in `sw.js`.
- It is published by GitHub Pages from `main`, root folder. Pushing `main` publishes it; there is no build and no workflow.
- The Android APK (a Capacitor wrapper, built by GitHub Actions) is retired. Its code lives in the `android-legacy`
  branch and is not updated. `index.html` still has a few lines that only run inside that wrapper (they look for
  `window.Capacitor`); they do nothing in a browser and can be deleted when convenient.
- Never commit signing keys or passwords. If an old signing key still sits in the repository secrets, it can be removed.

## Trying a change
1. Open `index.html` in Chrome. Most things work straight from the file; the microphone and MIDI need
   `localhost`.
2. For `localhost`, run the small server in the parent folder (`Pocket dev server.command`, or
   `node dev-server.js`). It shows the three apps and the branch each one is on.
3. Look at it on a phone-sized window (about 390 × 844): the apps are designed for phones first.
4. Open the browser console. A change is not done until it shows no red errors.
5. Run the automated checks (a real Chrome driven by Selenium; about 30 s):
   `python3 -m venv .venv && .venv/bin/pip install -r tests/requirements.txt` once, then `.venv/bin/python tests/e2e.py`.
   They open every view and help page, feed the app hostile names, malformed songs and damaged saved settings, and check
   that nothing runs, nothing throws and the app still starts. `tests/e2e.py` is the same file in all three apps; add a
   test there when you fix a bug. `node tests/midi-device.test.js` checks the MIDI helpers.

## Rules of thumb
- One file, no dependencies, no network (SING's lyrics search is the one exception). Keep it that way.
- Phones first: big squares, nothing that needs a hover or a right click.
- Every new button gets a `title`, a line of help in English and Spanish, and a toast that says what it did.
- Saved data must keep loading. When you change a format, fill in what old files lack (as `loadData` does).
- Fonts and code must be free and open (the only third-party code is listed in the top comment).
- Small commits with clear messages; try each change on a phone before a release.
