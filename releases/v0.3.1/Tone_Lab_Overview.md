# Tone Lab — Product Overview

**Tone Lab** is a single-file, browser-native FM synthesizer, sound-design playground, and step sequencer. It runs entirely in HTML / CSS / JavaScript / Web Audio with zero dependencies, no build step, and no installation — open the file and you're making sound.

The project takes inspiration from classic FM hardware workflows but is not an emulation: every engine, filter, envelope, effect and modulator is implemented from scratch in plain JavaScript so the entire signal path can be inspected, tweaked, and visualized in real time.

---

## At a Glance

- **4 synthesis engines** (FM Tone, FM Drum, Wavetone, Swarmer)
- **3 analog-style filter topologies** (Curtis, Ladder, SEM) with a full ADSR filter envelope
- **Full ADSR amp envelope + AUX envelope** (with destination, amount and live visual feedback)
- **6 LFOs** with BPM-synced speed, depth, amount, shape and 30+ destinations
- **Effects chain**: Overdrive → Chorus → Ping-Pong Delay → Plate / Spring Reverb
- **64-step sequencer** (4×16) with scales, transpose, retrigger, vertical-randomization, randomize-steps and **Elektron-style param locks**
- **A/B Morphing**: Octatrack-style scene morph between two patches
- **Real-time visualizers**: waveform, FFT spectrum, algorithm diagram
- **Patch JSON export / import** with full state preservation
- A **discrete tip strip** in the top-right of the header that cycles through 12 sound-design tips

---

## Layout

```
┌─────────────────────────────────────────────────────────────────────────┐
│ Tone Lab v0.1.0   ‹ tip … ›       (Device Tabs)                          │
├─────────────────────────────────────────────────────────────────────────┤
│ Engine    │  SYN1  │  SYN2  │  Filter+Amp+LFOs+AUX  │  Master(+FX)        │
│  └── Transport (Play / BPM)                                              │
│  └── Randomize box                                                       │
│  └── Symmetrize box                                                                                                                                       │
├─────────────────────────────────────────────────────────────────────────┤
│  Right pane: Waveform · Spectrum · Algorithm · Morphing                  │
├─────────────────────────────────────────────────────────────────────────┤
│  Bottom: Sequencer 4 × 16 steps + RETRIG · RANDOMIZE · MIGRATE ·         │
│          PARAM LOCK · TUNING · TRANSPOSE                                 │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Synthesis Engines

### FM TONE (D1 + D2)

A 4-operator FM voice modelled on the manual's algorithm appendix.

- **8 algorithms** with proper carrier/modulator topologies
- Operator levels, ratios, harmonic content (`HARM` bipolar −26…+26), inter-operator detune (`DTUN`), feedback (`FDBK`), and X/Y mix
- **Per-operator ratio offset** for fine detune
- **B-level macro** modelled on the hardware's "watch-hand" pattern with a small floor so RATIO B2 always has audible effect
- Operator envelopes (Atk / Dec / End / Lev) per A and B chains

### FM PERC (D2)

3-operator FM tuned for percussion: pitch sweep (STIM/SDEP), wavefolding, body envelope and bandpassed noise, with selectable operator base waves.

### WAVECORE (D2)

Two oscillators each addressing a wavetable (`PRIM` = sin/tri/saw/sqr crossfade, `HARM` = harmonic-stack crossfade), with phase distortion, ring/sync mod, detune and noise.

### HIVE (D2)

One main oscillator + 6 detuned swarm voices with selectable waveforms, animation, and noise modulation.

---

## Filter

Three filter topologies, each implemented as a per-sample TPT (zero-delay-feedback) state-variable filter so they're stable at high resonance:

- **Curtis** — smooth SSM-style 4-pole low-pass, well-behaved
- **Ladder** — Moog-style 4-pole, fattest sound, drops bass at high resonance
- **SEM** — Oberheim-style state-variable, more open and breathy, behaves nicely at high resonance

`CUT`, `RES`, `DRV` are continuous and per-sample modulatable. The filter envelope is full ADSR with a separate `AMT` (signed) and is applied per-sample with the filter's cutoff modulation, so envelope shapes track precisely.

A small **CTRL** snap pill on RES and DRV instantly snaps both to 0.

---

## Amp Envelope

Full ADSR (`ATK`, `DEC`, `SUS`, `REL`, `LEN`) with a live envelope canvas. The `LEN` parameter sets the gate-on length before release begins. Both `DEC` and `SUS` have **CTRL** snap pills (snap to 0.10) so you can quickly re-tighten a sound when polyphonic sequences start to blur.

---

## AUX Envelope

A second full ADSR envelope with its own destination dropdown (any synth knob, filter, FX param, or master vol) and a signed `AMT`. The destination is automated visibly: in playback the destination's slider literally moves with the envelope shape, plus a small "→ DEST 0.34" live indicator pulses in the AUX header.

---

## LFOs

Six LFOs, each with:

- **Shape**: sine, triangle, saw, square
- **Speed**: BPM-relative musical fractions (1/32 … 32 beats per cycle) — speeds are derived from the current BPM and re-derived live whenever BPM changes
- **Depth** (0..1) — sweep amplitude
- **Amount** (−1..+1) — bipolar scaling sent to the destination, with polarity invertible
- **Destination** dropdown including engine params, filter (cut/res/drv), filter envelope, AMP envelope, OD/Chorus/Delay/Reverb, master volume

LFO phase is **continuous across sequencer steps** (musically clock-synced via a `tStart` parameter that flows from `seqState.nextTime` into the renderer), so a "1/4" LFO completes exactly one cycle per quarter-beat regardless of which step is firing. A small live bar at the right of each LFO row shows real-time output so you can see each LFO is alive.

---

## Effects Chain

Fixed signal path: **Overdrive → Chorus → Ping-Pong Delay → Plate / Spring Reverb**.

- **Overdrive** — asymmetric soft-clip with HP at 80 Hz, drive 2× to 30×, tone tilt EQ (LP 800 Hz – 12 kHz). Equal-power dry/wet mix means MIX=1 is fully overdriven.
- **Chorus** — 3 voices, LFO depth up to ±15 ms, voices panned hard for stereo width, light feedback for resonance.
- **Ping-Pong Delay** — independent wet L/R buffers so MIX is independent of FB; soft-clipped output to keep peaks polite at high feedback.
- **Reverb** — switchable Plate / Spring. The Plate uses a Schroeder-Moorer comb+allpass network. The Spring is a physically-inspired tank model: 110/130-stage all-pass cascades per spring with modulated read taps, cross-coupling between channels, transformer saturation, and midrange band-pass shaping.

Every FX block has an ON/OFF pill that fires an instant re-render so changes are heard immediately.

---

## Sequencer

A 4×16 step grid (64 steps total). Default `LEN`=16 (only the first row is heard); inactive rows still hold values that other features can use.

Each step is a single cell with:

- A **value** (MIDI note or rest) and a **note label** (e.g. `C4`)
- **Top-left RED dot** → scope RETRIG to this step
- **Top-right GREEN dot** → scope RANDOMIZE STEPS to this step
- **Bottom-left PURPLE dot** → scope MIGRATE to this step (and ENABLES MIGRATE for that step even if global MIGRATE is OFF)
- **Bottom-right GRAY square** → toggle PARAM LOCK edit mode for this step

Click step value: ↑ semitone. Shift+click: ↓ semitone. Alt+click: rest.

Header sub-boxes:

- **RETRIG** (red) — global ON/OFF + HITS (2/3/4/6/8) + PROB. With per-step dots set, retrigger fires only on dotted steps. Without dots, retrigger fires globally. Each retrigger schedules N hard-cut hits with a per-hit gain envelope so you hear a true roll, not overlapping notes.
- **RANDOMIZE** (green) — PROB select + a Randomize Steps button. With per-step dots set, only dotted steps are reseeded; the rest of the sequence is preserved.
- **MIGRATE** (purple) — vertical randomization. With this probability, each step's playback note is picked from a random row at the same column position, so rows 2/3/4 act as a column-locked variation pool. Per-step dots scope it (and override the global toggle).
- **PARAM LOCK** (gray) — see below.
- **TUNING** (sky-blue) — sequencer length, scale, root note (with 14 scales including modes, blues, pentatonic, chromatic).
- **TRANSPOSE** (yellow) — ±1 semitone, ±1 octave on the entire sequence.

---

## Param Locks (Elektron-style)

Click any step's gray bottom-right square to enter PARAM LOCK edit mode for that step. The square pulses gold, the cell outlines in gold, and the synth sliders **snap to that step's locked values** (or to the global value where no lock exists).

Move any slider — across the SYN engine params, the filter (cut/res/drv), the filter envelope (amt/atk/dec/sus/rel), the AMP envelope (atk/dec/sus/rel/len), or any FX (OD, Chorus, Delay, Reverb) — and the value is recorded as a per-step lock. The locked tile lights up gold with a pulsing "L" badge.

**Multi-step recording** — click squares on several steps and slider tweaks now write the same lock value to every selected step at once. The header banner shows `EDITING 3 STEPS: 5,7,11` etc.

A header **CLEAR ALL** button wipes all locks across the sequence.

Locks are stored sparsely (`{stepIndex: {paramKey: value}}`) and applied non-destructively at render time: `state.params` is snapshotted, locks are written on top, the note renders, then the snapshot is restored. The audio engine, LFOs, AUX, FX chain and global state are completely unaffected.

---

## Per-Param Marks: RANDOMIZE & SYMMETRIZE

Each synth parameter tile has two badges:

- **R** (top-left, green) — mark the param for **RANDOMIZE**: hit `RAND` in the box on the left and every R-marked param gets a fresh random value across its full range
- **S** (bottom-left, cyan) — mark the param for **SYMMETRIZE**: hit `SYM` and every S-marked param gets a random value snapped to the nearest **even integer** for more consonant outcomes

R and S are **mutually exclusive** per param. SYMMETRIZE is suppressed on RATIO OFFSET params (where "even" is meaningless).

---

## Morphing

A right-pane MORPHING box with `SAVE A`, `SAVE B`, `CLEAR` buttons and a horizontal slider. Save a patch as scene A, change anything, save as scene B, then drag the slider to crossfade smoothly between them. Numeric params interpolate; categorical params (algorithm, waveform, filter type) snap at the midpoint. Both scenes ride along in the patch JSON.

---

## Visualization

The right pane carries three live canvases plus a section box:

- **Waveform** — the rendered note buffer in time domain, with Y-scale and cycle-zoom controls
- **Spectrum** — Cooley-Tukey radix-2 FFT magnitude plot in dB with adjustable floor
- **Algorithm** — SVG diagram of the current FM algorithm with feedback hooks and X/Y output arrows; click the diagram cycles through the 8 algorithms
- **Morphing** — A/B scene morph control

The waveform and spectrum re-render whenever any audio-affecting parameter changes — including R-marks, S-marks, sym/rand button presses, and live LFO output during playback.

---

## Hints / Tips Strip

A discrete inline strip at the top of the header cycles through 12 production-oriented tips when you click ‹ ›:

1. ROUNDED TONES — integer ratios for consonance
2. MODULATOR LEVELS — start low for warmth
3. CARRIER vs MODULATOR — what each role does
4. HARM PARAMETER — bipolar behavior
5. FDBK — sine-to-saw bend with a noise ceiling
6. ALGORITHMS 1 & 2 — when to use each
7. RATIO OFFSET = DETUNE — chorus-like beating
8. FILTER TYPES — Curtis vs Ladder vs SEM
9. AMP ENVELOPE — pluck vs pad recipes
10. MORPHING — A/B crossfade
11. RANDOMIZE WORKFLOW — R-mark + RAND
12. SEQUENCER SCALE — quantize randomized melodies

---

## Patch I/O

- **Init** — reset to the engine's default patch
- **Export** — download the entire app state as a Tone Lab JSON: device, machine, params, AMP/FILT/DLY/REV/OD/CHORUS state, LFOs, AUX, sequencer steps + scale/root + RETRIG/MIGRATE/RANDOMIZE/locks/dots, MORPH scenes, BPM, master volume, note Hz
- **Import** — load any Tone Lab JSON; auto-switches device/machine if needed and warns if the patch's machine isn't supported on the current device

The app boots with a baked-in `BOOT_PATCH` (a 16-step C-minor pentatonic-ish melody with SEM filter @ 0.38 cut, 0.6 res, plate reverb, 60 BPM, master 0.75) so you have something to listen to immediately.

---

## Architecture / Implementation Notes

- **Single HTML file**, ~6700 lines (~217 KB).
- **Web Audio API** for output, but every sound, envelope, modulation and effect is computed in JavaScript using `Float32Array` buffers — no AudioWorklets, no built-in nodes for the sound engine itself. The Web Audio API is used only as the output medium (via `AudioBufferSource` → `GainNode` → destination).
- **Per-sample modulation** for engine params, filter cutoff/resonance, and operator envelopes. **Render-time modulation** (with LFO + lock + AUX wiring) for the rest.
- **Cross-step LFO sync** via a `tStart` parameter that flows from `seqState.nextTime` into `renderFMTone` and `applyCurtisWithEnv`, so the LFO phase is continuous across notes.
- **Non-destructive state model** — `state.params` is the global patch, `seqState.locks` is a sparse per-step override map, MORPH stores two snapshots. Locks are applied via snapshot/restore around each note render so the global state is never permanently mutated by the sequencer.
- **Element-id discipline** — every slider, button, select, dot has a stable id so JSON export/import round-trips cleanly.
- **Build tag** — every release sets `<meta name="tonelab-build" content="…">` so users can confirm via DevTools that they're seeing the latest build (no service worker cache games).

---

## Browser Compatibility

Tone Lab targets modern Chromium / WebKit / Gecko (any browser shipped in the last 3 years). It uses:

- ES2020 syntax
- `Web Audio API`, `AudioBuffer`, `BufferSource`, `GainNode`
- `Canvas 2D` for waveform/spectrum/envelope drawing
- CSS variables + grid + `:has()` (for some scoped rules)
- `requestAnimationFrame` for live indicators

No installation, no permissions, no telemetry, no network calls.

---

## Demo-Ready Checklist

- [x] Boots into a playable patch immediately
- [x] All synth engines have presets
- [x] All effects work and re-render instantly when toggled
- [x] All 6 LFOs work, BPM-synced, applied to 30+ destinations
- [x] AUX envelope visibly automates its destination slider
- [x] Per-step features (RETRIG, MIGRATE, RANDOMIZE, PARAM LOCK) all working with multi-select for locks
- [x] Sequencer has scales + transpose + scope dots
- [x] Patch JSON export/import round-trips full state including locks and morph scenes
- [x] Waveform + spectrum + algorithm diagram all live-update
- [x] Tip strip cycles through 12 production tips
- [x] All slider controls have CTRL snap pills where relevant
- [x] Build version visible at top of UI and in `<meta>` tag
