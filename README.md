![SING.Pocket](.github/banner.png)

![SING.Pocket on five screens, each in a different theme](.github/readme/hero.png)


# SING.Pocket

Sing is the result of me not doing my vocal exercises daily enough. Singing has brought me a lot of happiness, and I kinda wanted something to have an excuse to practice more technique and less carpool karaoke. This is an amateurs idea of a vocal coach app, so bear with me. It kinda does what I need right now, and I hope you find it useful.

SING.Pocket is a pocket vocal coach: warm-ups that follow your voice, song practice with key and speed change, A-B loops, lyrics, a pitch test that shows where you sang in tune and where you didn't, recorded takes, a range finder, autotune, a sampler for your voice and a singing game. It is the sister app of [TRACK.Pocket](https://github.com/utopick/track-pocket) and [TUNE.Pocket](https://github.com/utopick/tune-pocket), and runs from one self-contained HTML file, with no install and no dependencies.

It should run fully offline, although opening the HTML file directly can cause microphone permission issues, so use the hosted GitHub page instead or self-host the HTML.

The one exception is the **lyrics search**: you don't need a connection to use the app, but searching for lyrics (FIND) won't work offline. Without a connection you can still paste lyrics or open an `.lrc` file.

**Open it:** [utopick.github.io/sing-pocket](https://utopick.github.io/sing-pocket/) · **Install it:** it is a web app (PWA): on Android, Chrome ▸ **Install app**; on iPhone, Safari ▸ Share ▸ **Add to Home Screen**; on a computer, the install icon in the address bar. Once installed it opens like an app and works offline.

I've been adding features and tweaking a lot, so they come out not fully tested, and there could be bugs. Leave me an issue and I'll fix it.

There is a help wiki inside (también en español), with small interactive demos you can drag around: a warm-up lane, the range finder, autotune and a pocket-size plane flight.


<p align="center"><img src=".github/readme/demo.gif" width="320" alt="SING.Pocket in use: a warm-up following the voice, a flight of the plane, the sampler and live autotune"></p>

## Why did I release these apps? 

Because GitHub allows me to host them in a GitHub site if I make them public, and I don't want to host them myself. 

Also because my friends told me that they are useful and some of you may want a free, ad-less gadget to pass the time.


## Ads and cookies

Yeah, no.

No ads, no cookies, no nothing. This is free and clean. No hidden stealers, no weird intentions.

Why is it free? Because not everything should be a product.

![A strip of SING.Pocket screens](.github/readme/strip.png)

## AI use

I'm a programmer and I have been coding for a living for over two decades, but life has given me too little free time that I want to spend offline and outside, making music and art. So I used a lot of AI to build this in a quick and easy way.

I'm not going to take credit for the code, even though I started this app from my own older code. I built it with the help of an AI coding assistant (Claude, by Anthropic), which wrote and reworked much of the code following my designs and notes.

The app itself uses no AI: everything runs on your device, and nothing you play, sing or record is sent to any AI service or anywhere else for that matter. The one exception is the lyrics search: when you tap **FIND**, the song's title and artist go to [lrclib.net](https://lrclib.net). If anything else leaves your phone, just shame me publicly and I'll fix it.

I did waste a lot of tokens by making it code it in a non-minified way, so if the bubble bursts and we go back to the basics it could be maintainable. 

If you want to try something made by me by hand (95%, I did use an early model to find some bugs but solved them by hand) in which I made the code, assets, music and all, check out Nanaya, my Pico-8 tower defense game: https://www.lexaloffle.com/bbs/?tid=153336

![alt text](.github/readme/nanaya.png)



## How to run it

Open `index.html` in a modern web browser. The microphone needs `https://` or `localhost`, which is why the hosted page (or the installed app) is the easy way.

## The basics

- **Warm-up:** 8 exercises (lip trill, 5-note scale, arpeggio, octave jump, staccato, siren, 9-note scale, hum) for six voice types or your own range. Each one climbs a semitone at a time through your range and back down. The lane shows the target notes and your live pitch, and every key gets a score.
- **Song:** open an MP3, M4A, WAV, OGG, FLAC or MP4 from the phone.
  - **KEY:** transpose −12 to +12 semitones without changing the speed.
  - **SPEED:** 50–120% without changing the pitch. Both use a WSOLA time stretch done in the app.
  - **KARAOKE:** turns the centre of the mix (usually the voice) down.
  - **Loops:** tap the overview to jump, drag it (or use SET A / SET B) to loop a part.
  - **LYRICS:** search lrclib.net (this needs a connection), paste them, or open an `.lrc` file.
  - **Pitch test:** **●** records you singing along and scores each lyric line. Tap a weak line to loop it.
- **Range:** hold your lowest and highest comfortable notes. A chart puts your range beside the six voice types and your earlier measures, and **USE MINE** fits the warm-ups and the game to it.
- **Autotune:** live through headphones (natural or hard, every note or a key), with a trace of your voice and of what you hear, and tuned copies of any recording.
- **Sampler:** for your voice, with 16 pads. Record a verse after a count-in, overdub harmonies on it, crop it, reverse it, change its pitch, and bake in effects (reverb, echo, double, chorus, third and fifth harmonies, octaves, autotune, radio, lo-fi…). Samples stay on the phone, and **SAVE** sends one to Files.
- **Plane:** a paper plane flies through hoops, and your voice steers it. The hoops follow a warm-up exercise in your range, on three levels (Easy forgives the octave and helps you aim). It's a warm-up in disguise.
- **History:** days in a row, minutes a day, your warm-up and song scores over time, your flights and your range measures.
- **Files** (last button): your songs and recordings. Play a take, open it in Autotune, export it (compressed, Opus or AAC), or delete it. **●** here records a free session.

![The views: warm-up, song, range, sampler, the plane game and autotune](.github/readme/views.png)

### Themes

Six looks, from **THEME** at the bottom of the Files screen: Dark, Light, Terminal, Dungeon synth, DMG and Demoscene. All three Pocket apps share them.

![A sing-along in the six themes](.github/readme/themes.png)

### About the melody

The reference melody is estimated from the full mix: the centre of the stereo image, band-passed to the voice range, with an autocorrelation pitch tracker. Loud instruments can fool it. Load an a cappella (vocal-only) version of the song with **VOCAL REF**, and that file becomes the melody you sing against.

### About YouTube

The app does not download from YouTube. Downloading is against YouTube's terms of service, and its embedded player doesn't let other apps change the pitch. Use songs you have as files.

Why is this in the readme? Well, me and the model, we had a long token consuming talk about pros and cons. He won, so BYOM.

### Your songs and takes

Songs, takes and Sampler pads are stored in your browser (IndexedDB), settings in `localStorage`; they are not uploaded anywhere. Clearing browser storage removes them, so export the takes you want to keep.

## Install it (PWA)

SING.Pocket is a progressive web app: the hosted page can be installed and then runs on its own, offline. It is `index.html` plus a tiny service worker (`sw.js`, which keeps the app files for offline use), a manifest and two icons. Nothing else is downloaded, and nothing is uploaded.

- **Android:** open the page in Chrome and choose **Install app** (or **Add to Home Screen**). The first time you use the microphone, allow it when the browser asks.
- **iPhone / iPad:** in Safari, Share ▸ **Add to Home Screen**.
- **Computer:** Chrome or Edge show an install icon in the address bar.
- **Updates:** the app looks for a new version whenever it is online; close and reopen it to get it.
- **MIDI keyboards and pads:** Chrome, Edge and Chrome on Android can use them (Web MIDI). Safari and iPhone cannot. The **MIDI** panel in the options lists what the browser sees, has an assign wizard and a TEST that shows what each key sends.
- Your data lives in the browser's storage for that site (separate from other sites and from any older Android APK). Export what you want to keep before clearing browser data.

**The Android APK is retired.** It was a Capacitor wrapper of this same page; the web app now does the same job without a build or an install from outside the store. Its code is kept in the [`android-legacy`](https://github.com/utopick/sing-pocket/tree/android-legacy) branch and the old APKs stay in [Releases](https://github.com/utopick/sing-pocket/releases); neither is updated any more.

## Files

- `index.html` — the complete app (markup, styles, and audio code)
- `midi-device.js` — the MIDI panel (device drawings, assign wizard)
- `manifest.webmanifest`, `sw.js`, `icons/` — what makes it installable and offline
- `docs/` — notes for whoever maintains it; `tests/` — automated checks (`tests/e2e.py`, Selenium) and a test for the MIDI helpers
- `LICENSE` — license terms

## License

SING.Pocket is released under the MIT License (see `LICENSE`).

AI assisted coding uses the uncredited work of a lot of coders through the years (me included) so anything I make that's not for work its as free as possible.

Have fun.

### Third-party code

The banner, app icons and favicons are lettered in [Inter](https://rsms.me/inter/) (SIL Open Font License 1.1), converted to shapes, so no font file is included. The app itself has no third-party code.

## The readmes

Yeah. They are all similar. I copied and pasted. No one ever reads them all. Kilroy was here.


Anyway, listen to me: You matter. A lot. I don't know you stranger, but you have my admiration and love. Keep making art.