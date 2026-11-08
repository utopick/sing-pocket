# SING.Pocket

A pocket vocal coach in one HTML file: warm-ups that follow your voice, song practice with key and speed change,
A-B loops, lyrics, a pitch test that shows where you sang in tune and where you didn't, and recorded takes.
It is the sister app of [TRACK.Pocket](https://github.com/utopick/track-pocket) and
[TUNE.Pocket](https://github.com/utopick/tune-pocket), and shares their look, six themes, animated backdrop and help manual.

Open `index.html` in a browser (the microphone needs `https://` or `localhost`), or install the Android APK.

## Views

- **Warm-up**: 8 exercises (lip trill, 5-note scale, arpeggio, octave jump, staccato, siren, 9-note scale, hum) for
  six voice types. Each one climbs a semitone at a time through your range and back down. The lane shows the target
  notes and your live pitch, and every key gets a score.
- **Song**: open an MP3, M4A, WAV, OGG, FLAC or MP4 from the phone.
  - **KEY**: transpose −12 to +12 semitones without changing the speed.
  - **SPEED**: 50–120% without changing the pitch. Both use a WSOLA time stretch done in the app.
  - **Loops**: tap the overview to jump, drag it (or use SET A / SET B) to loop a part.
  - **LYRICS**: search lrclib.net, paste them, or open an `.lrc` file.
  - **Pitch test**: **●** records you singing along and scores each lyric line. Tap a weak line to loop it.
- **Takes** (last button): your songs and recordings. Play a take, export it as WAV, or delete it. **●** here
  records a free session. The bottom bar has the pitch-test options, the theme and the help.

### About the melody

The reference melody is estimated from the full mix: the centre of the stereo image, band-passed to the voice range,
with an autocorrelation pitch tracker. Loud instruments can fool it. Load an a cappella (vocal-only) version of
the song with **GUIDE**, and that file becomes the melody you sing against.

### About YouTube

The app does not download from YouTube. Downloading is against YouTube's terms of service, and its embedded player
doesn't let other apps change the pitch. Use songs you have as files.

## AI use

I built this app starting from my own older code, with the help of an AI coding assistant (Claude, by Anthropic), which wrote and reworked much of the code following my directions and ideas.

The app itself uses no AI: everything runs on your device, and nothing you play, sing or record is sent to any AI service. The only network request it makes is the optional lyrics search on lrclib.net.

## Privacy and storage

Everything stays on the device: songs and takes in IndexedDB, settings in localStorage. The app makes one kind of
network request, the lyrics search on lrclib.net, and only when you tap FIND. Uninstalling the Android app deletes
its songs and takes, so export the takes you want to keep.

## Android APK

The app is wrapped with Capacitor 6 in `android/`. GitHub Actions builds a debug APK on every push and on demand
(Actions → Android APK → Run workflow); download it from the run's artifacts.

To build locally (Node 20, JDK 17 and the Android SDK):

```sh
cd android
npm ci
npm run android:init   # once: creates android/native and adds the microphone permissions
npm run android:apk    # android/native/app/build/outputs/apk/debug/app-debug.apk
```

The microphone works because the manifest declares `RECORD_AUDIO` and `MODIFY_AUDIO_SETTINGS` and Capacitor
asks for them at runtime. The app also doesn't force an audio sample rate, which some phones refuse.
