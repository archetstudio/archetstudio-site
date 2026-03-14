# MetroTune — Feature Extraction & Marketing Reference

> **Internal document.** For App Store copy, press materials, and product descriptions.
> Tempo Feedback system is implemented but **not production-ready** — exclude from all
> public-facing materials until a future release explicitly enables it.

---

## Feature 1 — Engine Precision

### Technical Reality
MetroTune runs a dual-path audio engine. The primary path uses `react-native-audio-api`
(a native Web Audio API implementation) with `AudioContext` at 44,100 Hz. WAV click files
are pre-decoded into `AudioBuffer` objects at startup; each tick fires
`createBufferSource().start()` from a pre-decoded buffer — no disk read, no decoder
latency at playback time. On Android, this routes through the MMAP/Oboe path, bypassing
AudioFlinger's AudioTrack stack entirely. The tick scheduler uses absolute wall-clock
timestamps with drift correction: each interval is computed from the last scheduled time,
not from `Date.now()` at callback entry, so JS timer jitter never accumulates. BPM range:
20–240, with 11 classical tempo markings (Grave → Prestissimo).

### Marketing Spin
Every click arrives exactly when you expect it. MetroTune's audio engine is built on the
same Web Audio specification used by professional browser DAWs, running through Android's
lowest-latency Oboe audio stack — the same path used by professional recording apps. No
click is ever re-decoded at runtime. What you hear is a pre-loaded waveform fired from
memory, on a schedule that self-corrects for any delay. At 240 BPM with 16th-note
subdivisions, every one of those 16 clicks per second lands precisely where music theory
says it should.

### Competitive Edge
Most metronome apps use `AVAudioPlayer`/`SoundPool` with a cold `play()` call per tick,
which carries 5–50ms of unpredictable latency depending on device load. MetroTune's
scheduler is drift-corrected against absolute time, not relative intervals — so a 30-minute
practice session never drifts. A Samsung-specific MMAP patch addresses a known
hardware bug that causes audible timing irregularities in competing apps on affected devices.

---

## Feature 2 — Tuner Algorithm

### Technical Reality
Pitch detection uses the **McLeod Pitch Method (MPM)** via `pitchy` v4.1.0 — normalized
autocorrelation with parabolic interpolation for sub-bin frequency resolution. Buffer:
2,048 samples at 44,100 Hz. Detection range: **60–2,000 Hz** (covers all orchestral
instruments, from bass guitar low E through piccolo). Effective pitch resolution: ~0.1 Hz
at concert pitch via parabolic interpolation. Clarity gate: 0.8 (rejects uncertain
detections). Frequency is EMA-smoothed (α=0.3) before note mapping. **A4 reference pitch
is user-adjustable 420–460 Hz in 1 Hz steps.** Sharp/flat display follows pitch trend
direction — rising pitch shows the sharp name, falling pitch shows the flat enharmonic.
During active metronome playback, pitch detection is gated off for the duration of each
click sound + 50ms, preventing the metronome from registering as a false pitch reading.

### Marketing Spin
MetroTune's tuner uses the same pitch detection method published in peer-reviewed
acoustics research — the McLeod Pitch Method — not the simple FFT approximation that
causes the "needle flickering" in cheaper tuner apps. Your violin's open A string rings at
exactly 440 Hz; MetroTune resolves pitch to within a tenth of a hertz. If your orchestra
tunes to A=442 (common in European ensembles) or A=432, change the reference in a single
tap — it recalibrates the entire cents scale instantly. And if you're practicing with the
metronome on, the tuner knows the difference between your instrument and the click.

### Competitive Edge
Standard chromatic tuners use a single FFT pass with ~21 Hz/bin resolution. MPM's
parabolic interpolation resolves ~0.1 Hz at concert A — roughly a 200× improvement in
raw frequency precision. The A4 calibration range (420–460 Hz) covers baroque tuning,
standard 440, European orchestral 442–444, and early-music 430 — competing apps typically
offer only 435–445 or a fixed 440.

---

## Feature 3 — 24-Hour Premium Unlock (Rewarded Ad)

### Technical Reality
A single Unix timestamp (`"premium_unlock_timestamp"`) is stored in `AsyncStorage`.
On app mount it reads this value and computes `remainingMs = storedTimestamp + 86,400,000
− Date.now()`. If positive, premium is active — **entirely offline, no server check.**
A 60-second interval re-checks expiry but never phones home. When the rewarded ad
completes, `Date.now()` is written to `AsyncStorage`. If premium expires mid-session,
the app gracefully downgrades the active sound and voice type. The premium gate covers
13 percussion sounds, 4 tuner voice profiles, and adjustable pitch checker parameters.

### Marketing Spin
A full professional toolkit — 13 studio percussion sounds, four instrument-modeled
reference tones, and advanced tuning settings — available for exactly zero cost, in
exchange for watching one short video. The 24-hour unlock is stored on your device: once
activated, it works in airplane mode during a two-hour rehearsal, a transatlantic flight,
or an outdoor recital with no signal. No account, no subscription, no reminder emails.
Just play.

### Competitive Edge
Many "free with ads" music apps lock features behind a hard paywall or require an
internet connection to validate premium status. MetroTune's offline-first approach means
the premium window functions identically with or without network connectivity — a
meaningful advantage for musicians who practice in studios, basements, or performance
halls with poor signal.

---

## Feature 4 — Resource Efficiency & Privacy-First Audio

### Technical Reality
All microphone data is processed in the 2,048-sample buffer callback and discarded — no
audio is written to disk or transmitted at any point. The tuner's oscillator voice pool
(3 × 8-oscillator chains) is permanently connected to `AudioContext.destination` at
initialization; there are zero `connect()`/`disconnect()` calls during playback,
eliminating the Oboe audio graph recompilation penalty on Android that causes audible
pops in competing apps. The metronome does not use a background audio session or wake
lock — if backgrounded, the scheduler detects clock-drift on return and snaps forward
cleanly instead of firing catch-up bursts. On Android 14+, `FOREGROUND_SERVICE_MICROPHONE`
is declared, meeting the new foreground service subtype requirements.

### Marketing Spin
MetroTune never records your music. Every note you play through the tuner is analyzed
and discarded in the same fraction of a second — it never touches storage, never leaves
your phone. The app is also honest about what it needs: it won't keep the metronome
running in your pocket draining battery, and it won't claim audio access when you switch
away. On Android 14, it declares its audio purpose explicitly at the operating system
level — microphone for tuning, nothing else.

### Competitive Edge
Apps that connect/disconnect audio nodes on every note trigger Android's Oboe graph
recompiler, causing 5–20ms audio pops audible during rapid note sequences. MetroTune's
permanently-wired node graph eliminates this at the architecture level. The
`pendingCleanup` promise pattern also prevents a race condition that causes intermittent
microphone conflicts on iOS — a known crash class in apps that rapidly start/stop
`AVAudioSession`.

---

## Feature 5 — Sound Library (19 Sounds + Additive Synthesis Profiles)

### Technical Reality
38 WAV files (19 sounds × normal + accent variants) bundled locally — no asset download
at runtime. Free tier: click, wood, hi-hat, kick, beep. Premium tier: deepwood, rosewood,
bamboo, keyboard, marimba, xylophone, deepclick, thud, snap, cowbell, rimshot, claves.

The tuner's 5 synthesized voice profiles use **additive synthesis** based on real acoustic
instrument harmonic series:

| Profile | Harmonic Series | Character |
|---------|----------------|-----------|
| Tone | Fundamental only | Pure sine |
| Clarinet | Odd harmonics [1,3,5,7,9,11,13,15], 1/n² rolloff | Hollow, woody |
| Organ | Hammond-style [1,2,3,4,6,8] | Full, rich |
| Brass | Sawtooth-like 1/n rolloff [1–8] | Bright, buzzy |
| Bell | Risset inharmonic partials [1.0, 2.0, 0.56, 1.19, 2.74, 3.0, 4.07, 1.71] | Metallic shimmer |

Vibrato is applied via LFO at 5 Hz, 2-cent depth, with delayed onset: 400ms for Clarinet,
300ms for Brass — matching how live players introduce vibrato after the initial attack.

### Marketing Spin
When you're tuning on stage before a concert, you want a reference tone that lives in the
same sonic space as your instrument. MetroTune's Clarinet profile generates its reference
by adding the exact same odd harmonics that make a real clarinet sound like a clarinet —
not a buzzy approximation. The Bell profile uses Risset's inharmonic partial series, the
mathematical model behind why bells ring the way they do. Play a low C on your instrument,
tap the keyboard, and your tuner reference no longer fights with your instrument's overtones.

### Competitive Edge
Nearly all mobile tuner reference tones are a single sine wave or a basic sawtooth —
spectrally opposite to most instruments' natural timbre. MetroTune's additive synthesis
profiles are derived from acoustic instrument physics. For string players, a reference
tone that shares harmonic structure with the instrument makes tuning by ear significantly
faster and more accurate.

---

## Feature 6 — Pitch Checker (Active Intonation Confirmation)

### Technical Reality
`usePitchChecker` is a requestAnimationFrame-based state machine monitoring the live
tuner pitch stream. When detected pitch stays within a configurable cent tolerance of a
target note, a hold timer begins. On expiry, it fires a confirmation in one of three modes:
- **Click** — fires the currently selected metronome percussion sound
- **Beep** — fires the beep sound
- **Echo** — plays the confirmed note back through the tuner's oscillator voice pool at
  the configured instrument profile (Tone, Clarinet, Organ, Brass, or Bell) for a
  configurable duration

During echo playback, pitch detection is suppressed to prevent the reference tone from
triggering a false new detection.

Configurable parameters:

| Setting | Free | Premium |
|---------|------|---------|
| Tolerance | ±10 cents fixed | ±5–20 cents adjustable |
| Hold duration | 500ms fixed | 250–3,000ms adjustable |
| Confirmation sound | Beep only | Click, beep, or echo with instrument profile |

Visual feedback: a quadrant-based progress ring fills clockwise on the CHECK button
during hold. On confirmation, the note letter flashes the active accent color and scales
1.15→1.0.

### Marketing Spin
MetroTune listens to your intonation and tells you when you've nailed it. Hold a note in
tune — the circle fills — and a confirmation click, beep, or the note itself played back
in your chosen instrument voice signals success. It's ear training and intonation practice
built into the tuner, not a separate app. Set the tolerance as tight as ±5 cents for
advanced work, or as generous as ±20 cents for beginners building confidence. A student
learning to sustain a pure A on a new instrument hears the difference between "close" and
"confirmed" without a teacher in the room.

### Competitive Edge
No major free metronome/tuner app on the App Store or Google Play offers active
intonation confirmation feedback. Competing tuners (GuitarTuna, PitchLab, Cleartune) are
passive displays — they show you a needle, you read it. MetroTune closes the feedback
loop: the app tells you *when* you've succeeded, with a sensory signal you can receive
without reading the screen. The echo mode — replaying the confirmed pitch in a Clarinet
or Bell timbre — is also an ear training tool: you sustain the note, the app plays it
back, and you compare live against your instrument.

---

## Feature 7 — Simultaneous Metronome + Tuner

### Technical Reality
The metronome engine (`useMetronome`) and the tuner pipeline (`useTuner`) run as
completely independent hooks in the same React component tree, sharing only a single
`AudioContext` instance. Neither hook stops or suspends the other when activated. The
one coordination point: pitch detection is gated off for each metronome click + 50ms
(`suppressUntilRef`) to prevent false pitch readings — but the tuner continues running
during this window. On iOS, a single `playAndRecord / defaultToSpeaker` audio session
supports simultaneous input (microphone for tuning) and output (metronome click through
speaker).

### Marketing Spin
Other apps make you choose: metronome or tuner. MetroTune runs both at the same time.
Start the metronome, flip to the tuner tab, and play — your instrument's pitch is detected
in real time while the click continues in your ear. It's how you actually practice: you
don't stop the beat to check if your E is sharp, you check while keeping time. The tuner
ignores the metronome click and focuses on your instrument. No interruptions, no mode
switching.

### Competitive Edge
This is a direct, verifiable gap in the market. Pro Metronome — one of the top-ranked
apps in the category — stops the metronome when the tuner is opened and vice versa.
Metronome+ separates the two into different apps entirely. MetroTune's architecture makes
the features genuinely concurrent, not tab-switched. For a student doing intonation work
at tempo, or a chamber musician checking tuning while keeping a reference click running,
this is a workflow difference that competing apps physically cannot offer without
architectural rework.

---

## Feature 8 — Vocal Beat Counting with Instrument Layering

### Technical Reality
Voice mode overlays spoken beat-count audio on top of the active percussion sound using
a split-role mixing strategy: when voice is active, percussion fires on subdivision
positions only, while voice WAV files fire on beat positions — no double-triggering on
downbeats. Both tracks play simultaneously through the same `AudioContext`.

The voice asset library:
- **Counting words:** `voice-one.wav` through `voice-twelve.wav` (supports up to 12-beat measures)
- **Downbeat accent:** dedicated `voice-one-accent.wav` — a distinct, emphasized
  pronunciation of "one" for beat 1 of each measure
- **Subdivision syllables:** and, e, a, trip, let — mapped to correct rhythmic positions
  for 8th notes, 16th notes, and triplet patterns

Two voice genders available. Male voice is synthesized from the same female recordings
played at `playbackRate = 0.75` — no separate asset required. Male voice is premium-gated;
female voice is free.

### Marketing Spin
MetroTune counts with you. Enable voice mode and instead of a click on every beat, you
hear "one — two — three — four" spoken aloud, with "ONE" emphasized on the downbeat of
every bar. Add 16th notes and you hear "one-e-and-a, two-e-and-a" — the syllables music
teachers have used for generations to internalize subdivisions. The percussion click stays
active on every subdivision the voice doesn't cover, so you always have a sonic anchor on
the off-beats. Choose a female or male counting voice based on whichever sits better in
your ear against your instrument.

### Competitive Edge
Most voice-mode metronomes offer counting on the main beat only, and play voice *instead
of* the click — creating silence on subdivisions. MetroTune's voice and percussion operate
on separate rhythmic roles simultaneously: voice names the beats, click fills the
subdivisions. A student practicing 16th notes at 80 BPM hears both "two-e-and-a" spoken
and four distinct clicks underneath — a layered reference that neither channel alone
provides. The dedicated `voice-one-accent.wav` reinforces measure structure in a way that
simply repeating the standard "one" file cannot.

---

## Feature 9 — Sustained Reference Drone

### Technical Reality
The 12-note tuner keyboard supports two tap modes. A short tap plays the note for a
minimum of 1,000ms then releases. A **hold gesture (≥1,000ms)** activates sustained mode:
the note plays continuously for up to **60 seconds** via a dedicated third oscillator
voice set, reserved exclusively for sustained notes and separate from the short-tap pool.
The note auto-stops at 60 seconds to prevent indefinite resource hold.

During sustained playback, vibrato engages after a delay — 400ms for Clarinet, 300ms for
Brass — matching how live players introduce vibrato after the initial attack. The
sustaining keyboard chip displays a looping float animation as a visual indicator. The
octave selector (0–8) remains fully interactive during sustained playback, and a **lock
toggle** prevents the auto-follow feature from changing octave while the drone is held.
The sustained voice set is managed independently of the short-tap pool, so tapping other
notes for quick reference does not interrupt the sustained note.

### Marketing Spin
Hold any note on the keyboard and MetroTune becomes a drone machine. Press and hold C4,
then pick up your instrument and play — a pure reference tone, or a resonant Clarinet,
Organ, Brass, or Bell timbre, sustains in the background for a full minute while your
hands are free. Use it to tune a string by ear against a sustained fifth. Use it to
practice intonation on a single pitch without watching a screen. Use it alongside the
metronome — start the click, hold a reference A, and practice a scale passage with both a
rhythmic and a pitch reference running simultaneously. The note holds. You play.

### Competitive Edge
Competing tuner apps require you to keep your finger on the screen to sustain a reference
tone — releasing the button stops playback immediately. MetroTune's hold-to-sustain
gesture hands the note off to a dedicated voice channel that runs independently of touch
input, freeing both hands. Combined with simultaneous metronome operation (Feature 7),
a musician can have a sustained drone, a running metronome, and live pitch detection all
active at once — a combination that requires three separate apps in most other workflows.

---

## Summary Table

| Feature | Free | Premium | Competitor Gap |
|---------|------|---------|----------------|
| MMAP/Oboe low-latency engine | ✓ | ✓ | Most apps use AudioTrack |
| MPM pitch detection, 0.1 Hz resolution | ✓ | ✓ | FFT-only in competitors |
| A4 reference 420–460 Hz (1 Hz steps) | ✓ | ✓ | Narrow or fixed range |
| Simultaneous metronome + tuner | ✓ | ✓ | Others stop one for the other |
| Voice counting (female) + subdivision syllables | ✓ | ✓ | Click-replacement only elsewhere |
| Sustained reference drone (60s, hands-free) | ✓ | ✓ | Finger-held only elsewhere |
| Pitch Checker — active intonation confirmation | Basic | Full | Not available elsewhere |
| 24h offline premium unlock (rewarded ad) | — | ✓ | Paywall or online-only |
| Voice counting — male voice | — | ✓ | — |
| Premium percussion library (13 sounds) | — | ✓ | — |
| Instrument synthesis profiles (Clarinet, Organ, Brass, Bell) | — | ✓ | Sine-only elsewhere |

---

*Last updated: 2026-03-02*
