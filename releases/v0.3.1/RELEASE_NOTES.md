# Tone Lab v0.3.1

Frozen snapshot — this directory contains the exact files served as the
public live build of Tone Lab v0.3.1. Do not edit. Future work happens
on the working copy in the parent directory under v0.4.0-dev.

This is a small follow-up to v0.3.0. The functional surface is the
same — the patch is mostly polish, naming hygiene, and a few refinements
that landed after the v0.3.0 cut.

## What's new since v0.3.0

- All "digitone" naming removed from the live source (HTML, README,
  and Tone_Lab_Overview). The historical references in
  `releases/v0.2.1/` and `releases/v0.3.0/` are intentionally
  preserved as frozen records of those releases.
- Tone_Lab_Overview.md naming cleaned up and shipped at the repo root
  for the first time.
- iPad / narrow-viewport refinements (build tag also reflects this:
  `v0.3.0-dev-ipad-ready+naming-clean` was the dev build that became
  v0.3.1-stable).

The full v0.3.0 feature set is still in effect and described in
`releases/v0.3.0/RELEASE_NOTES.md`. Headlines for context:

- D3 device family with the **PL** macro-oscillator (16 model slots,
  Plaits-inspired). See `LICENSES.md` for the Mutable Instruments / MIT
  attribution.
- Iwato default scale; AMP DEC/SUS = 0.5; FILTER CUT/RES/DRV = 0.75 / 0 / 0.
- ROOT BIAS slider (bell ↔ uniform; deterministic root at left).
- Waveshaper FX block before Overdrive in the master chain.
- Dedicated MORPH LFO inside the LFO panel (7th row, lime).
- Filter NORM toggle and AMP env squared-curve resolution.
- Live LFO + FILTER ENV slider animation on destination params.
- Soft limiter on master output (DynamicsCompressorNode).
- Keyboard shortcuts (P / F / W / O / C / D / R).
- Sequencer vertical collapse (chevron in title bar); SYN1 / SYN2 /
  FILT-AMP / MASTER horizontal collapse with auto-redistribute.
- Viewport-fit auto-scaling for iPads / narrow windows.
- LFO transport-freeze when stopped; LFO 64× speed option.
- ENGINE box recolored periwinkle (#a5b4fc).
- Many bug fixes: RANDOMIZE STEPS scope, MIGRATE eligibility, RETRIG
  eligibility, MORPHING volume drop, MIDI EXPORT colors, MASTER bullet,
  scale notes fixed width, and others.

## Build identifier

  tonelab-build = v0.3.1-stable
