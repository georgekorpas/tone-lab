# Tone Lab v0.2.1

Frozen snapshot — this directory contains the exact files served as the
public live build of Tone Lab v0.2.1. Do not edit. Future work happens
on the working copy in the parent directory under v0.3.0-dev.

## What's in this release

- 4-engine architecture: FM TONE (D1/D2), FM PERC, WAVECORE, HIVE (D2-only).
- Sequencer: 4×16 grid, LEN 1..64, scales/root, transpose ±semi/±octave,
  C1..C7 note range, RETRIG, RANDOMIZE, MIGRATE (with per-step purple
  dots + visual source-flash), PARAM LOCKS with multi-step editing.
- Filter (Curtis / Ladder / SEM) with NORM (RMS makeup gain).
- Full ADSR envelopes for filter / amp; AUX envelope with destination.
- 6 BPM-synced LFOs (sine / tri / saw / square) with destinations.
- FX chain: overdrive, chorus, ping-pong delay, plate/spring reverb.
- A/B morph fader with linear param interpolation.
- Patch I/O (JSON), MIDI Export (.mid + .txt) with steps field.
- Visualizers: waveform, spectrum, algorithm diagram, morph.

## Build identifier

  tonelab-build = v0.2.1-stable

