# Tape Sketch

A pocket tape deck for getting musical ideas down fast. Set a tempo, pick a drum pattern, record up to three takes over it, and export a WAV. It runs in the browser as an installable app (PWA), works offline once installed, and keeps everything on your phone.

The look is a worn four-track: wood trim, scratched panel, gaffer tape, labels in sharpie.

## What it does

- **Tempo and loop length:** 40 to 240 bpm, 1 to 8 bars, tap tempo, metronome.
- **808-style drums:** synthesized in the browser, no samples. Eight voices: kick, 2nd kick, snare, clap, hat, open hat, hi tom, crash.
- **Drum pattern:** 2 bars of 16th notes (32 steps), edited on its own screen. Eight presets to start from: Four on the floor, Boom bap, Trap, Rock, Funk, Half-time, Disco, Minimal.
- **Drive:** a saturation knob on the drum bus.
- **Three audio takes:** looper-style. Arm a take, press Rec, and it records one pass of the loop over the playing pattern, then plays back.
- **Tape window:** a lane for each take showing its waveform, plus a lane for the drum pattern, with bar lines and a moving playhead. Tap a take's lane to select it, tap the drum lane (or its Drums tape) to open the drum screen.
- **Nudge:** with a take selected, the left and right buttons shift it earlier or later in 1 ms steps (hold to repeat, and it speeds up the longer you hold), up to 300 ms either way. The shift is stored per take, applies to playback and export, and can be reset.
- **Reverb and volume** on every strip, including the drums.
- **Ideas list:** every idea autosaves with its title, pattern, settings and takes.
- **Export:** a stereo 16-bit WAV mixdown of one pass of the loop plus a short reverb tail.

## Using it

1. Set the tempo and the number of bars first. Recorded takes keep their length, so changing either later will not stretch them.
2. Open **Edit drums**, pick a preset with the arrows or by tapping its name, and change any steps you like. Tap a drum name to hear that sound.
3. Press **Play**. Turn on **Click** if you want a metronome.
4. Press **Rec**. It arms the first empty take, waits for beat 1, records one loop, then plays it back. Use the red button on a strip to choose a specific take. The cross clears it.
5. Shape the mix with the knobs (Drive, Verb) and faders (volume). Drag a knob up or down to turn it.
6. Check your take in the tape window. If it sits a little behind or ahead of the drums, tap its lane and use the nudge buttons until the waveform lines up with the drum hits. Press play to hear it as you adjust.
7. Tap the title on the masking tape to rename the idea. Open **Ideas** to switch between ideas or start a new one.
8. **Export WAV** saves a mixdown to your downloads.

## Getting the best recordings

- Use wired headphones. Bluetooth adds too much delay for playing along.
- Press the gear icon to set the **latency offset**. Takes are shifted earlier by this amount so they land on the beat. If takes sound late, raise it. If they sound early, lower it. Use the nudge buttons for a single take, and the settings offset when every take is off the same amount.
- Echo cancellation, noise suppression and auto gain are switched off for the microphone so music is not processed.
- With speakers instead of headphones, the drums will bleed into the microphone.

## Putting it online (GitHub Pages)

The microphone only works over HTTPS, so the app needs to be hosted.

1. Create a **public** repository, for example `tape-sketch`.
2. Upload the contents of this folder so that `index.html` sits at the top level of the repo, with the `icons` folder beside it.
3. In the repo go to **Settings, Pages**, set the source to **Deploy from a branch**, choose `main` and `/ (root)`, and save.
4. After a minute or two the app is at `https://YOUR-USERNAME.github.io/tape-sketch/`.

### Installing on Android

1. Open the link in Chrome while online and let it finish loading.
2. Open the Chrome menu and choose **Install app** (or **Add to Home screen**).
3. Launch it from the home screen once while online so the files and the sharpie font are cached. After that it works offline.
4. Allow microphone access the first time you press Rec.

### Updating

Upload the changed files, and change `VERSION` in `sw.js` (for example to `tape-sketch-v2`) so installed copies pick up the new files. You may need to close and reopen the app once or twice to see the update.

Saved ideas live in the browser under the app's exact address. If you rename the repo or move to a different URL, the new address starts empty.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole app: interface, audio engine, storage |
| `manifest.webmanifest` | Name, colours and icons for installing |
| `sw.js` | Service worker that makes it work offline |
| `icons/` | App icons (192, 512 and a maskable 512) |

## How it works

- **Audio:** the Web Audio API. Drums are built from oscillators and filtered noise. A lookahead scheduler keeps the steps in time.
- **Recording:** an AudioWorklet captures the microphone with sample-accurate start and end points tied to the loop, then trims the take by the latency offset.
- **Reverb:** one shared convolution reverb with a send level per strip.
- **Export:** the mix is rendered offline with the same graph used for playback.
- **Storage:** ideas are saved in IndexedDB on the device. Nothing is uploaded anywhere.

## Known limits

- Nudging a take earlier trims its start, and nudging later leaves a gap at the start. Large shifts leave silence at one end of the loop.
- Takes do not time-stretch if you change the tempo or bar count after recording.
- There is no count-in before recording starts.
- Export renders one pass of the loop, not a full arrangement.
- Drum voice levels and the latency estimate are starting points and may need adjusting for your phone.
- Only tested for Chrome on Android.
