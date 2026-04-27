# Tone Lab — User Manual

**▶ Live demo: https://georgekorpas.github.io/tone-lab/**

A single-file in-browser FM synthesizer, sound-design playground and step sequencer. Open the live demo above in any modern browser (Chrome, Safari, Firefox, Edge) — no installation, no build step, no network calls.

This manual walks the entire interface from the simplest concepts down to the deepest features, in the order the controls actually appear on screen.

---

## 1. Two device modes — DN1 and DN2

Tone Lab models two distinct sound-engine families, switched at the top-right of the header:

- **D1** — only the FM TONE engine is available. The original 4-operator FM voice.
- **D2** — adds three more engines on top of FM TONE: **FM PERC**, **WAVECORE** and **HIVE**.

Switching device only changes which engines are *available*; your current patch (the slider state) stays put if the chosen machine still exists. Otherwise it falls back to FM TONE.

The version pill `v0.1.0` next to the title shows the build, and the discrete tip strip in the header centre cycles through 12 sound-design tips when you click ‹ ›.

---

## 2. Engine — what it is and how to use it

The leftmost box on the screen is the **ENGINE** box. Inside, you pick which synthesis engine the whole patch uses.

### The four engines

- **FM TONE** — 4-operator FM synthesis with 8 algorithms. Bell-like tones, electric pianos, brass, bells, plucks, pads. The "main" tonal engine, available on both D1 and D2.
- **FM PERC** *(D2 only)* — 3-operator FM tuned for percussion. Punchy kicks, snares, hats, toms, claps.
- **WAVECORE** *(D2 only)* — two oscillators, each pointing at a wavetable (`PRIM` = sin/tri/saw/sqr crossfade, `HARM` = harmonic-stack crossfade), with phase distortion, ring/sync modulation, detune and noise.
- **HIVE** *(D2 only)* — one main oscillator + 6 detuned swarm voices. Wide pads, supersaw-style leads.

Click any of the four buttons to switch — the SYN1 / SYN2 boxes immediately repopulate with that engine's parameters.

### How the parameters are organised

After picking an engine you'll see two boxes labelled **SYN 1** and **SYN 2**. They split the engine's parameters across two pages:

- **SYN 1** typically holds the "core" sound-shaping controls (operator ratios, harmonic content, feedback, mix).
- **SYN 2** holds the operator envelopes or the second-page controls (operator level/decay/end shape, oscillator detune, etc.).

Right-click any parameter tile to mark it for **RANDOMIZE (R, top-left green badge)** or **SYMMETRIZE (S, bottom-left cyan badge)** — see the per-param marks section below.

---

## 3. Preset — loading, stepping, saving fresh

Inside the ENGINE box, below a faded chartreuse separator, sits the **PRESET** section:

```
[ ‹ ] [ ▼  Preset name  ] [ › ]
[       INIT (yellow)       ]
[ ⤓ Export ] [ ⤒ Import ]
```

- **‹ / ›** — step backwards or forwards through the dropdown's preset list. Wraps around at the ends.
- **▼ Preset dropdown** — pick any preset by name; loads on selection. The list changes per engine.
- **INIT** — black with a chartreuse border + glow. Resets the patch to that engine's clean starting point.
- **⤓ Export** — downloads the entire current state (engine, params, sequencer, locks, morph scenes, FX, BPM, master vol) as a Tone Lab JSON file.
- **⤒ Import** — load a previously exported Tone Lab JSON; auto-switches device/machine if needed.

Each engine ships with curated presets:

- **FM TONE**: Init, Pure Sine, FM Saw, FM Square, Bell, Tubular Bell, EPiano (DX), EPiano (Soft), Rhodes MK1, Brass, Bass, Pluck, Wood, Glass, Strings, Pad, Organ, Sub Bass, Marimba, Muted Pluck.
- **FM PERC**: Init, Kick, Snare, HiHat, Open Hat, Tom, Clap, Cowbell, Rim Shot.
- **WAVECORE**: Init, Pure Sine, Pure Triangle, Pure Saw, Pure Square, Pulse 25%, Saw Stack, Detuned Saws, Ring Mod, Sync Lead, Hiss.
- **HIVE**: Init, Wide Saw, Choir, Square Swarm, Octave Down, Soft Pad, Subtle.

All presets are tuned for moderate, professional output with controlled noise.

---

## 4. Transport — Play, Stop, BPM

Below the ENGINE box sits **TRANSPORT** (bright purple identity, pulsing LED).

- **PLAY** — black button with a bright purple outline + text + halo. Click to start the sequencer. The button's text and outline turn red while the sequencer is running and now reads "STOP". Clicking it again stops everything.
- **SEQ toggle** — informational pill (sequencer mode is now Elektron-style always-on; the toggle is reserved for future use).
- **BPM** — purple-glowing number cell. Type a value (40–300) or click and drag.

The Transport box subtly pulses with a soft purple halo so you can always find PLAY at a glance.

---

## 5. Randomize and Symmetrize boxes

Below the Transport box are two related controls:

### RANDOMIZE box (mark-pink)

```
N marked
[ RAND ] [ UNMARK ]
```

Right-click a parameter tile inside SYN1/SYN2 to flip its top-left **green R** badge on. The counter goes up. Hit **RAND** and every R-marked parameter gets a fresh random value across its full range. **UNMARK** clears all R marks at once.

### SYMMETRIZE box (cyan)

```
N marked
[ SYM ] [ UNMARK ]
```

Same workflow but using the bottom-left **cyan S** badge. SYM rolls a random *even* integer for each S-marked param (snapped to the param's own grid), producing more consonant outcomes. R and S are mutually exclusive per parameter. SYM is suppressed on RATIO OFFSET params (where "even" is meaningless).

### MIDI EXPORT box (orange)

```
STEPS [  16  ]
[ MIDI ] [ TXT ]
```

Dumps the **pure note pattern** of the sequencer to disk so you can play it on another device — a hardware sequencer, a DAW track, an external synth's arp, etc. Only notes are exported: no parameter locks, no automation, no per-step dots, no FX state.

- **STEPS** — number of steps to write (1..64). Defaults to the active sequencer length on load. The exported file is exactly this long. Steps beyond your current pattern are written as rests, so you can pad a 16-step idea out to 32 if needed.
- **MIDI** — downloads `tonelab_seq_<timestamp>.mid`, a Standard MIDI File (format 0, single track, MIDI channel 1, velocity 100, PPQ 480). Each step is one 16th-note tick at the file's nominal grid; **no tempo meta is baked in** so the destination device sets its own BPM. Most DAWs and hardware sequencers will simply re-time the steps to whatever tempo they're running.
- **TXT** — downloads `tonelab_seq_<timestamp>.txt`, a plain-text dump (one line per step: `step  midi  noteName`, rests shown as `---`). Handy for visually checking the pattern or feeding into tools that don't speak SMF.

---

## 6. SYN 1 — the engine's main controls

The exact controls depend on the chosen engine. For **FM TONE** they are:

| Control | What it does |
|---|---|
| **RATIO C** | Frequency ratio of operator C (typically the carrier). |
| **RATIO A** | Frequency ratio of operator A. |
| **RATIO B1 / B2** | Frequency ratios of the two B-chain operators. |
| **HARM** | Bipolar (-26 … +26). Negative enriches operator C with upper partials. Positive enriches A and B1 (modulators) — usually a much bigger sound change. |
| **DTUN** | 0..127 detune. Slight detune between A and B chains for a chorus-like beating. |
| **FDBK** | Operator self-feedback (0..120). Bends a sine toward sawtooth shape. Small amounts (10-40) add bite; above 80 quickly becomes noisy. |
| **MIX** | Bipolar (-64 … +63) crossfade between the algorithm's X and Y outputs. |

Below the row are **PAGE 2 — RATIO OFFSET** params (`OFF C / A / B1 / B2`): tiny floats in -1..+1 for fine detune (chorus shimmer at 0.01-0.05; clavinet/EP shimmer at ~0.2).

---

## 7. SYN 2 — operator envelopes & secondary controls

For **FM TONE** this page holds the per-operator envelopes:

- **ATK A / DEC A / END A / LEV A** — attack, decay, end-level and overall level of operator A's envelope.
- **ATK B / DEC B / END B / LEV B** — same for the B chain (B1 + B2). LEV B drives a "watch-hand" macro that morphs between B1 and B2 levels.
- **ALGO** — algorithm 1..8 (clickable diagram in the right pane).

**Scroll up / down on any slider** to fine-step. **Double-click the value text** to type an exact value. **Right-click the tile** to toggle the R/S marks.

---

## 8. Filter — Curtis / Ladder / SEM

Above the AMP envelope you'll find the **FILTER** section, with its three topology buttons in the header:

- **Curtis** — smooth SSM-style 4-pole low-pass. Resonant but well-behaved.
- **Ladder** — Moog-style 4-pole. Fattest, drops bass at high resonance.
- **SEM** — Oberheim-style state-variable. More open and breathy, behaves nicely at high resonance.

The three sliders:

- **CUT** — cutoff frequency, exponentially mapped 20 Hz to ~20 kHz.
- **RES** — resonance (0..0.95). Has a `CTRL` snap pill that resets it to 0.
- **DRV** — pre-filter drive (0..1). Has a `CTRL` snap pill that resets it to 0.

### NORM toggle (new!)

In the filter header, next to the topology buttons, sits the **NORM** pill. When active, the filter applies post-filter RMS makeup gain so the perceived loudness stays roughly constant as you sweep CUT and RES. Turn it on when you want to hear timbral changes without the volume jumping around.

The makeup gain is capped at ×8 boost so closing the cutoff completely doesn't blow up.

---

## 9. Filter envelope — full ADSR

Below the filter sits a dedicated **FILTER ENVELOPE** panel with:

- **ON / OFF** pill toggle.
- **AMT** — bipolar amount (-1..+1). How much the envelope pushes the cutoff up (or down) from its slider position.
- **ATK / DEC / SUS / REL** — full ADSR with shape preview canvas.

The envelope is applied per-sample with the cutoff modulation, so envelope shapes track precisely.

---

## 10. AMP envelope — full ADSR with CTRL snaps

Below the filter envelope is the **AMP ENVELOPE**:

- **ATK / DEC / SUS / REL / LEN** — full ADSR plus a `LEN` parameter that sets the gate-on length before release begins.
- Both **DEC** and **SUS** have **CTRL snap pills** that snap them to `0.10` — a quick way to pull a buzzy patch back into a tight pluck shape.
- A live envelope canvas shows the current shape.

Curated presets keep DEC and SUS in the 0.05-0.20 range so polyphonic sequences don't pile up into a wash. Long ring is shaped by the *operator* envelopes (decA/decB/endA/endB) instead.

---

## 11. AUX envelope

Below AMP is the **AUX ENVELOPE** — a second full-ADSR envelope with its own destination dropdown. It can target any synth knob, filter param, FX param, or master vol.

- **ON/OFF** — top-right pill.
- **ATK / DEC / SUS / REL / AMT** — standard ADSR plus a signed `AMT` (-1..+1).
- **DEST** — dropdown of every modulatable destination.
- **Live indicator** — header pulse + text "→ DEST 0.34" updates in real time.
- **Visual feedback** — the destination's slider literally moves with the envelope shape during playback, so you see what AUX is automating.

---

## 12. LFOs — six modulators, BPM-synced

Below AUX is the **LFOs** panel — six independent low-frequency oscillators. Each row:

```
[#] [shape] [SPEED ▼] [DEPTH 0%] [AMT +0%] [DEST ▼] [live bar]
```

- **Shape**: sine ∿, triangle △, saw ◢, square ▭.
- **SPEED** — BPM-relative musical fractions (1/32 … 32 beats per cycle). Speeds re-derive live whenever BPM changes.
- **DEPTH (0..1)** — how much the LFO sweeps (its own output amplitude).
- **AMOUNT (-1..+1)** — how much that swing reaches the destination, with polarity (negative inverts).
- **DESTINATION** — dropdown of every modulatable destination.
- **Live bar** at the right shows the LFO's real-time output, even when nothing is playing — a visual sanity check that the LFO is alive.

LFO phase is **continuous across sequencer steps** (BPM-clock-synced via `tStart`), so a "1/4" LFO completes exactly one cycle per quarter beat regardless of which step is firing.

---

## 13. Master + Effects chain

The rightmost box is **MASTER** (mint-teal, with a pulsing LED). Inside, in this fixed order:

1. **VOL** — master volume (0..1).
2. **OVERDRIVE** — DRV / TONE / MIX. Asymmetric soft-clip with HP at 80 Hz.
3. **CHORUS** — RATE / DEPTH / MIX. 3 voices, hard-panned, light feedback.
4. **PING-PONG DELAY** — TIME / FB / MIX. Independent wet L/R buffers.
5. **REVERB** — Plate or Spring, with SIZE / DAMP / MIX. Plate uses a Schroeder-Moorer comb+allpass network; Spring is a physically-inspired tank model with all-pass dispersion chains.

Every FX block has an ON/OFF pill that fires an instant re-render so changes are heard immediately.

---

## 14. Right pane — Visualizers

The right side of the screen has four panels:

- **WAVEFORM** — time-domain plot of the current note buffer, with Y-scale and cycle-zoom controls (red identity).
- **SPECTRUM** — FFT magnitude in dB with adjustable floor (sky-blue identity).
- **ALGORITHM** — clickable SVG diagram of the current FM algorithm (fuchsia identity). Each click cycles to the next algorithm.
- **MORPHING** — see next section.

All canvases re-render whenever any audio-affecting parameter changes.

---

## 15. Morphing — A/B scene fader

In the right pane, below Algorithm, sits **MORPHING** (lime-green identity):

```
[ SAVE A ] [ SAVE B ] [ CLEAR ]
A —————◆——————— B          25% · A↔B
A ↔ B · live morph active
```

Save the current full patch as scene **A**, change anything, save as scene **B**. Drag the slider to crossfade smoothly between them.

Numeric parameters interpolate linearly. Categorical params (algorithm, waveform, filter type) snap at the midpoint. Both scenes ride along in the patch JSON.

---

## 16. Sequencer — 4 × 16 steps

The full-width box at the bottom is the **SEQUENCER**. 64 steps total, arranged as 4 rows × 16 columns. Default `LEN`=16 (only the first row plays).

### A single step

Each cell shows:

- A **note value** (MIDI note like `C4`) or a **rest** (—) and the step number.
- **Top-left RED dot** → scope RETRIG to this step.
- **Top-right GREEN dot** → scope RANDOMIZE STEPS to this step.
- **Bottom-left PURPLE dot** → scope MIGRATE to this step. Acts standalone — works even with global MIGRATE off.
- **Bottom-right GRAY square** → toggle PARAM LOCK edit mode for this step.

**Editing notes:** click step value to step ↑ a semitone, Shift+click to step ↓, Alt+click to make it a rest. Right-click also steps down. The full available range is **C1 (MIDI 24) to C7 (MIDI 96)** — clicks past either end are clamped, never wrapped.

The two octave buttons in the TRANSPOSE sub-box transpose the entire pattern up or down by 12 semitones, and Shift on the existing ±SEMI buttons does ±octave. All transposes are clamped to C1..C7.

### Header sub-boxes (left → right)

#### RETRIG (red)

Per-step burst repeater.

- **ON / OFF** toggle (red pill).
- **HITS** — 2× / 3× / 4× / 6× / 8× hits per trigger.
- **PROB** — probability of retrigger firing.

If any per-step red dot is set, RETRIG fires only on dotted steps. With no dots, it can fire on any step. Each retrigger plays N hard-cut copies with a per-hit gain envelope so you hear a clean roll, not overlapping notes. A red dot on a step is enough to enable retrigger for that step even when global RETRIG is OFF.

#### RANDOMIZE (green)

Step-value re-roller.

- **PROB** — chance per step.
- **Randomize Steps** — button.

Per-step green dots scope it: when any are set, only dotted steps are reseeded; the rest of the sequence is preserved. 15% of steps become rests for musicality.

#### MIGRATE (purple)

Vertical randomization. With this probability, the played note is taken from a randomly-chosen ROW at the current column position (column = step mod 16). Rows below LEN still count — that's the whole point.

- **ON / OFF** toggle (purple pill).
- **PROB** — a purple slider with a live `0%..100%` readout. Sets the probability that a migration roll succeeds on every eligible step.

**Eligibility rule (simple):**

- Global **MIGRATE ON** → applies to **all** steps. Per-step purple dots are ignored.
- Global **MIGRATE OFF** → applies **only** to steps with a purple dot lit. Everything else plays its written note.

In both cases the PROB roll then gates whether the swap actually happens on a given trigger. The stored sequence is never mutated; the swap is "live" per trigger.

#### PARAM LOCK (gray)

Elektron-style per-step parameter overrides. See §17.

- **STATUS** — `— off —` / `EDITING STEP` / `EDITING STEPS` / `N steps locked`.
- **CLEAR ALL** — button that wipes every lock.

#### TUNING (sky-blue)

- **LEN** — sequencer length 1..64.
- **SCALE** — pick from 14 scales (modes, blues, pentatonic, chromatic, etc.).
- **ROOT** — root note for randomization.

#### TRANSPOSE (yellow)

- **− SEMI / + SEMI** — transpose entire sequence ±1 semitone.
- **− OCT / + OCT** — transpose ±1 octave.

---

## 17. Param Locks — Elektron-style per-step overrides

Click any step's **gray bottom-right square** to enter PARAM LOCK edit mode for that step. The square pulses gold, the cell outlines in gold, and the synth sliders (SYN1/SYN2 + filter + AMP envelope + filter envelope + every FX param) **snap to that step's locked values** (or to the global value where no lock exists yet).

Move any slider to record a per-step lock for that parameter. The locked tile lights up gold with a pulsing "L" badge.

### Multi-step locking

Click multiple gray squares — sliders now write the same lock value to **every** selected step at once. The header banner reads `EDITING STEPS`. Clicking a square again removes that step from the recording set.

### What can be locked

- All SYN engine params (RATIO C/A/B1/B2, HARM, DTUN, FDBK, MIX, LEV A/B, operator envelopes, ratio offsets, algorithm).
- Filter cut / res / drv.
- Filter envelope (amt / atk / dec / sus / rel).
- AMP envelope (atk / dec / sus / rel / len).
- Overdrive (drive / tone / mix).
- Chorus (rate / depth / mix).
- Delay (time / fb / mix).
- Reverb (size / damp / mix).

Engines, LFOs, AUX envelope, MORPH state, sequencer steps and global state are completely unaffected by lock editing — every lock is applied via snapshot/restore around its single render call.

### Clearing

- **Click the same gray square again** to remove that step from edit mode.
- **CLEAR ALL** in the PARAM LOCK header sub-box wipes every lock across the entire sequence.

---

## 18. Patch I/O & persistence

Everything is preserved in patch JSON: device, machine, params, AMP/FILT/AUX/DLY/REV/OD/CHORUS state, all 6 LFOs, sequencer steps + scale + root + LEN + BPM, all per-step dot masks (RETRIG/RAND/MIGRATE), all PARAM LOCKS, MORPH scenes, master volume, note Hz.

- **⤓ Export** writes a `tonelab_<machine>_<preset>_<timestamp>.json` file.
- **⤒ Import** restores everything and warns if the patch's machine isn't supported on the current device.

The app boots with a baked-in default patch (a 16-step C-minor pentatonic-ish melody with SEM filter at 0.38 cut, plate reverb, 60 BPM) so you have something to listen to immediately.

---

## 19. Tips strip

A discrete inline strip in the centre of the header cycles through 12 sound-design tips when you click ‹ ›. Topics include rounded-tone recipes, modulator levels, carrier vs modulator, HARM behaviour, FDBK warnings, algorithm choice, ratio offset detune, filter type comparison, AMP envelope shapes, morphing, randomize workflow, and sequencer scale quantization.

---

## 20. Build version

The pill `v0.1.0` next to the title shows the major version. The browser-readable build tag (in DevTools → Elements → `<head>`) under `<meta name="tonelab-build">` shows the precise build identifier — useful for verifying you've reloaded after an update.

---

## 21. Browser & runtime

- **Modern Chromium / Safari / Firefox**, ES2020.
- **Web Audio API** for output only — every engine, filter, envelope, modulator and effect is computed in JavaScript using `Float32Array` buffers (no AudioWorklets, no built-in nodes for the sound engine itself).
- **No installation, no permissions, no telemetry, no network calls.**

Just open the file. That's it.
