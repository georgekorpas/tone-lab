# Tone Lab v0.3.0

Frozen snapshot — this directory contains the exact files served as the
public live build of Tone Lab v0.3.0. Do not edit. Future work happens
on the working copy in the parent directory under v0.4.0-dev.

## What's new since v0.2.1

- D3 device family with the **PL** macro-oscillator (16 model slots,
  Plaits-inspired, all 16 implemented). See `LICENSES.md` for the
  Mutable Instruments / MIT attribution.
- New **ROOT BIAS** slider in the seq TUNING box (bell ↔ uniform).
- New **Waveshaper** FX block before Overdrive in the master chain.
- Dedicated **MORPH LFO** inside the LFO panel (7th row, lime).
- Filter **NORM** toggle and AMP env squared-curve resolution.
- Live LFO + FILTER ENV slider animation on destination params.
- Soft limiter on master output (DynamicsCompressorNode).
- Keyboard shortcuts (P / F / W / O / C / D / R).
- Sequencer vertical collapse; SYN1 / SYN2 / FILT-AMP horizontal collapse.
- Viewport-fit auto-scaling for iPads / narrow windows.
- Iwato default scale; AMP DEC/SUS = 0.5; FILTER CUT/RES/DRV = 0.75 / 0 / 0.
- ENGINE box recolored periwinkle (#a5b4fc).
- Many bug fixes: RANDOMIZE STEPS scope, MIGRATE eligibility, RETRIG
  eligibility, MORPHING volume drop, LFO transport-freeze, MIDI EXPORT
  colors, MASTER bullet, and others.

## Build identifier

  tonelab-build = v0.3.0-stable
